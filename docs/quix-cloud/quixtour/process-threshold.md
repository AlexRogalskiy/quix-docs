# Process - threshold detection

In this part of the tour you'll learn how to create a threshold detection transform. The transform detects if CPU load exceeds a certain threshold, and if so, sends a message to its output topic.

## Create the threshold detection transform

To create the threshold detection transform:

1. In your `Develop` environment, click on `Code Samples` in the main left-hand navigation. 
2. Select the `Python`, `Transformation`, and `Basic templates` filters.
3. For `Starter transformation` click `Preview code`.
4. Click `Edit code`.
5. Name the transform "CPU Threshold".
6. Select the input topic `cpu-load`.
7. For the output topic, add a new topic called `cpu-spike`.
8. In the application view, click on `main.py` to edit it.
9. Replace all the code in `main.py` with the following:

    ``` python
    from quixstreams import Application
    import os

    # Create an Application
    # It will get the SDK token from environment variables to connect to Quix Kafka
    app = Application()

    # Define input and output topics
    input_topic = app.topic(os.environ["input"])
    output_topic = app.topic(os.environ["output"])

    # Create a StreamingDataFrame to process data
    sdf = app.dataframe(input_topic)

    # Filter in all rows where CPU load is over 25.
    sdf = sdf.filter(lambda row: row["cpu_load"] > 25)

    # Produce message payload with alert.
    sdf = sdf.apply(lambda row: "CPU value is " + str(row["cpu_load"]))

    # Print messages to the console
    sdf = sdf.update(lambda row: print(row))

    # Send messages to the output topic
    sdf = sdf.to_topic(output_topic)

    if __name__ == "__main__":
        # Run the Application
        app.run(sdf)
    ```

11. Tag the project as `process-v1` and deploy as a service.
12. Monitor the logs for the deployed process.

If CPU load exceeds the threshold the message is published to the output topic, for further processing in the next stage of the pipeline. 

## 🏃‍♀️ Next step

[Serve your data :material-arrow-right-circle:{ align=right }](./serve-sms.md)
