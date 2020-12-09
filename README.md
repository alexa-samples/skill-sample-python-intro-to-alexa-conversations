# Intro to Alexa Conversations 

This guide contains the Python code for the "Intro to Alexa Conversations" [template available in the Alexa Developer Portal](https://developer.amazon.com/en-US/docs/alexa/conversations/get-started.html), and provides a simple starting point to demonstrate a few key concepts in Alexa Conversations. Out of the box it supports the following voice flows:

 - A modal launch showing an APL welcome screen and APL-A welcome prompt ("open *conversation starter*")
 - A one-shot invocation that stores a favorite color in the skill session, repeating the stored color back to the user and showing the color in an APL (visual) template
 - An invocation asking for the favorite color within the same session, again showing the color on the screen and repeating the color back to the user

As a developer, you can see examples of:

 - Annotated dialogs to consume user input
 - Calling a configured API to pass the captured input to the Lambda function,
 - Storing the input in the skill session and returning a valid API response
 - Processing the response in APL-A (audio) and APL (visual)

Please refer to the [Alexa Conversations developer documentation](https://developer.amazon.com/en-US/docs/alexa/conversations/about-alexa-conversations.html) for additional details or clarifications on terminology used in this guide.

# AWS Lambda code

This guide assumes you will provision your backend code in AWS Lambda, so when following the "Get Started..." section of the [Developer Guide](https://developer.amazon.com/en-US/docs/alexa/conversations/get-started.html), make sure to choose the "Provision your own" option. Login to your AWS account and create a [Lambda function](https://console.aws.amazon.com/lambda/home?region=us-east-1#/create/function) using the following information:

## Select "Author from Scratch"

1. **Function Name**: Give your function a name, like __ConversationStarter__
2. **Runtime**: __Python 3.8__

Leave permissions and advance settings at their defaults, then click **Create function**.

## Create a deployment package

Create a deployment package to upload to AWS Lambda using the following steps.

1. Clone this repository
2. Change directories to the cloned files, and open the directory from the command line.
3. Install libraries in a new, project-local package directory with pip's `--target` option:

        ```
        $ pip install -r requirements.txt --target ./package
        ```

4. Create a ZIP archive of the dependencies

        ```
        $ cd package
        $ zip -r9 ${OLDPWD}/function.zip .
        ```

5. Add your function code (lambda_function.py) and utility code (util.py) to the archive.

        ```
        $ cd $OLDPWD
        $ zip -g function.zip lambda_function.py util.py
        adding: lambda_function.py (deflated 75%)
        adding: util.py (deflated 68%)
        ```

For further help creating a deployment package, see [AWS Lambda deployment package in Python](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html).

## Upload your deployment package

1. After creating your ZIP file, upload it to AWS Lambda by clicking the "Actions" dropdown and selecting "Upload a .zip file"

    ![](img/deployment.png)

2. Select the "function.zip" file you created in the previous step, then click **Save**.

## Granting Alexa access to your Lambda function

1. From the __Configuration__ tab your Lambda function, click the **Add Trigger** button.
2. Select "Alexa Skills Kit" as the trigger source
3. Add the skill ID of the skill you created in [https://developer.amazon.com/en-US/docs/alexa/conversations/get-started.html](https://developer.amazon.com/en-US/docs/alexa/conversations/get-started.html)
4. Click the **Add** button to save your trigger.

# Modifying the template

Before modifying the template to begin your development, you should try to following invocations and trace their path through the annotated dialog to understand how Utterance Sets, API invocations, API responses and Response Templates are connected together in the dialog to create the interaction and round trip. Begin with the following phrases:

 1. "Alexa, open *conversation starter*"
 2. "Alexa tell *conversation starter* my favorite color is blue"
 3. (within an existing session, after being prompted by Alexa "What would you like to do?") "what is my favorite color"

You can feel free to leave the existing dialogs, APIs, response templates and utterance sets in place and start building your own dialogs, just be aware that utterances that match those in the existing utterance sets have a chance of invoking those dialogs.

## To completely clear any traces of the template and start with a 'bare metal' Alexa Conversations skill

 - Delete the Utterance Sets named **GetFavoriteColor** and **SetFavoriteColor**
 - Delete the dialogs **RecordFavoriteColor** and **GetFavoriteColor**
 - Delete the API Definitions **RecordColor** and **GetFavoriteColor**
 - Delete the Response Templates **RecordColorSuccess**, **GetFavoriteColorSuccess** and **RequestFavoriteColor**
 - You will also want to modify the **welcome** template so that the modal skill launch (i.e. "open *conversation starter*) is more appropriate for your skill (if needed) and change the visual response template contents to something different instead of a page showing color selections.