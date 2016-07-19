# Check for Spam

> DotPlant2 implemented verification forms sent to the spam, which checks fields of forms and comments on the existence of a "suspicious" data and only sends scanned data alerts. This helps you to immediately discard "spam" and do not spend time on them.

To set up, follow these steps:

1. In the administrative section, select the property group which is responsible for verifiable form.

2. Edit the field "to interpret as" properties of the form as follows:

- "No" - then do not check box for spam
- "Username" - the field is responsible for the name of the sender
- "Content" - is contained in the message content

3. Go to "Settings/Config" tab. At the core is a group of settings tab E-mail. In the `Spam Checker Api Key` you need to select your preferred method of verification (["clean web"](https://tech.yandex.ru/cleanweb/) from Yandex and ["Akismet"](https://akismet.com) of WordPress).
4. Then go to "Settings/Check for spam", select the method of verification is included and specify the API key for it.

This spam protection setup is complete.