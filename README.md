# Jenkins job triggers closed PR webhook from Github

## Step 1
- In order to trigger a webhook from Github, we have to install [Generic Webhook Trigger Plugin](https://wiki.jenkins.io/display/JENKINS/Generic+Webhook+Trigger+Plugin).

## Step 2
- Create a pipeline job.

![Step 2](/img/step2.png)

## Step 3
- Check `Generic Webhook Trigger` option in `Build Triggers` tab.

![Step 3-1](/img/step3-1.png)

- Configure the option with these following properties:

![Step 3-2](/img/step3-2.png)

![Step 3-3](/img/step3-3.png)

![Step 3-4](/img/step3-4.png)

![Step 3-5](/img/step3-5.png)

![Step 3-6](/img/step3-6.png)

- Choose `Pipeline script` in `Pipeline` tab and paste the [script](/Jenkinsfile) into it.
- Note: `GenericTrigger` in the script is as same as above configurations.

![Step 3-7](/img/step3-7.png)

## Step 4
- Add webhook in your Github repositories.
- Payload URL: `https://ci.kobiton.com/generic-webhook-trigger/invoke?token=TriggerPR` (You can change other token parameter in Jenkins configuration as [Step 3](#step-3)).
- Content type: `application/json`.
- Check `Active` option.

![Step 4-1](/img/step4-1.png)

- Choose only `Pull requests` option in `Let me select individual events`.

![Step 4-2](/img/step4-2.png)
