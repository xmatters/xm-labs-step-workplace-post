# Post to Facebook Workplace step for xMatters
This is a custom step for xMatters Flow Designer that can post to a group in Facebook's Workplace using a preconfigured Bot.  Once you've created this custom step in a communication plan in xMatters you will be able to post to a Workplace group feed from any flow in that communication plan.

This is repo makes part fo the [xMatters Labs Flow Steps](https://github.com/xmatters/xMatters-Labs-Flow-Steps) parent repo.


---------

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</kbd>

---------

# Files

* [x_logo.png](/media/x_logo.png) - An xMatters logo in green that you can use to configure the bot in Workplace.
* [xmatters blue.png](/media/xmatters%20blue.png) - An xMatters logo in blue if you prefer that for the bot.
* [workplace.jpg](media/workplace.jpg) - Logo for the Workplace step in xMatters Flow Designer

# Workplace
[Facebook's Workplace](https://www.workplace.com/) brings all the neat stuff that we love Facebook to, well... the workplace.

I've seen a lot of xMatters customers take up this product recently.  Usually one of the first things they want to do is create a group in Workplace for Major Incidents so they can broadcast any current issues and demonstrate that the resolution is in hand.  Often customers then extend this to post information relevant to a particular team straight into their own group, like a callout, escalation or an FYI for an interesting service.

All this of course requires a way to automatically post to a Workplace group from xMatters so you don't have to take time out of your incident to copy and paste the details over.  Once you have this custom step in your communication plan you'll be able to do just that.

It may be that this step is the ultimate aim of your flow, or it may be that his is just one of the many things you want xMatters to do as part of your process.  Either way this is a really easy step to setup and a quick win.  Don't be put off by the idea of a custom step either.  This is very often someone's first custom step and it's not uncommon to hear "*that was easy*" just a few minutes after they open up Flow Designer.


# Create the Custom Step
Here is the configuration and script you will require to create the Workplace custom step.

On the **Developer** tab in xMatters, click **Edit** next to the communication plan you need this step in and then click **Flows** in the menu.  Open the Flow Designer for any of the forms/flows. On the **CUSTOM** tab in right had toolbar you will find the **Create a custom step** button, click it. Fill in the details as shown in the following sections.

Once you have the custom step created you'll be able to use it over and over again in any flow in this com plan.

## Post to Workplace Group Feed

### Settings

| Field | Value |
| ----- | ----- |
| Name | Post to Workplace |
| Description | Use this step to post a message to a Facebook Workplace Group Feed. For more details on this step and it's setup see the instructions on the xMatters GitHub repo xm-labs-step-workplace-post  |
| Icon | <kbd> <img width=70 src="/media/workplace.jpg"></kbd> |
| Include Endpoint | Yes |
| Endpoint Type | No Authentication |
| Endpoint Label | Workplace |

<img src="/media/Post to Workplace step - Settings.png" />

### Inputs

| Name  | Required? | Min | Max | Help Text | Default Value | Multiline |
| ----- | ----------| --- | --- | --------- | ------------- | --------- |
| Access Token | Yes | 0 | 2000 | Setup your bot in Workplace and it will give you a token to authenticate with, put that in here. |  | No |
| Message | No | 0 | 20000 | This is the message text to post to the group. You can use Workplace's markdown to format your message (https://www.facebook.com/help/work/541260132750354) |  | Yes |
| Link | No | 0 | 20000 | Have Workplace fetch a link and show it in the post. You can specify a Link and a Message at the same time. At least one has to be specified. |  | No |

<img src="/media/Post to Workplace step - Inputs.png" />

### Outputs

| Name |
| ---- |
| Created post ID |

<img src="/media/Post to Workplace step - Outputs.png" />

### Script

```javascript
console.log('About to post to Workplace, Message: ' + input['Message']);

var params = '';
if ( input['Link'] )  {
    params = 'link=' + input['Link'] + '&';
}
if ( input['Message'] )  {
    params = params + 'message=' + input['Message'] + '&';
}
if ( !params )  {
    console.log('ERROR - Either Link or Message is required, none were specified');
    throw 'ERROR - Either Link or Message is required, none were specified' ;
}

params = params + 'formatting=MARKDOWN' + '&access_token=' + input['Access Token'] ;

var request = http.request({
    "method": "POST",
    "endpoint": "Workplace",
    "path": "/v3.0/group/feed?",
    headers: {
        "Content-Type": "application/json",
    }
});

var response = request.write(params);

output['Created post ID'] = JSON.parse(response.body).id;
```



# Workplace setup
Now we're going to do a little work in Workplace.  xMatters needs a Workplace 'Bot' created in the group you want it to post to.  This not only gives xMatters all it needs to authenticate and post but also gives Workplace information on who is posting and how it should represent the post in the group.

## Create a Bot

<img width=90% src="/media/Workplace Bot Setup.gif" />

1. Point a new browser window to Workplace.  Find and open the group you want xMatters to post into.
1. On the **More** menu for the group you'll find **Integrations**, click it.

  Now you're looking at the Integrations configuration page for this particular group, anything you do here will only effect this group.

1. Find the **Create publishing bot** tile and click the **Create bot** button in it.
1. In the dialog that opens give your new bot a **Name**, **Description** and a **Profile image**.

  You can use one of the xMatters logos in the files list above for the image if you like.  Remember that when xMatters posts to your group it will post with this image and name so it's good if this represents the process you are creating.

1. Click **Create** and wait a moment while Workplace creates your bot and token.
1. You'll be presented a new dialog with 3 items on it, the last of these is the **Access Token**, click **Copy Access Token** and paste it somewhere safe.  You'll need this in just a moment.
1. Finally read the warning, check the box named **I understand** and click the **Done** button.

That's it, you've created a bot.  Congratulations!  You can come back into this Integrations section of your group at any time to see the bot, delete it or get a new Access Token.




# Configure the Step
Right, back in xMatters create a flow if you haven't already.  There's lots of help for this on [Flow Designer on the xMatters help pages](https://help.xmatters.com/ondemand/xmodwelcome/flowdesigner/laying-out-flow-designer.htm).

<img width=90% src="/media/Workplace Step Config in Flow Designer.gif" />

Then you can drag your shiny new custom step into the canvas and configure it.

- If you've already created the **Workplace Graph API** as an endpoint select it on the **ENDPOINT** tab.  If not then you can create it there with the **Base URL** as `https://graph.workplace.com`

- You'll need that **Access Token** [you just got from creating your bot](#create-a-bot).  You may consider putting the token in a Com Plan constant and then put the constant in this input instead.  This makes it a little easier to change later if needed especially if you end up using this in several steps.

- You can use as much [Workplace Markdown](https://www.facebook.com/help/work/541260132750354) as you like to format your **Message**.  You can also put hit return to put a new line in and this will be delivered as a new line to Workplace to help you create useful, easy on the eye posts.

- You must specify either a **Message** or a **Link**, or both.
