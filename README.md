# vtiger-sms-gateway-center
VTIGER SMS Notifier Extension with SMS Gateway Center

## SMS Gateway Center
Send SMS in India and International market. Signup and get free SMS credits to test our service at [SMS Gateway Center](https://www.smsgateway.center)

## About VTiger SMS Gateway Center 
You can install this plugin in SMS Notifier plugin and start sending SMS from your VTIGER CRM Application.

## Install via Console
```cd /var/www/html/vtigercrm/modules/SMSNotifier/providers```

```wget  https://github.com/smsgatewaycenter/vtiger-sms-gateway-center/blob/master/SmsGatewayCenter.php```

## Install via FTP

[Download the File](https://github.com/smsgatewaycenter/vtiger-sms-gateway-center/blob/master/SmsGatewayCenter.php)

Save the file to your local system and upload via FTP in your VTiger CRM App **Vtiger/modules/SMSNotifier/providers**

You should have installed SMS Message Module from the Modules Store.

* Click on the SMS Notifier tab. You can find it in All the drop-down menus in the menu bar.
* Click on the wrench icon > Server configuration
* Click on New configuration and configure the following details in the pop-up window that appears.
* Complete all the information requested in the form.

**Provider** : Let's select SmsGatewayCenter from the Provider's drop-down list.
**Active** : Lets check and activate SmsGatewayCenter provider.
**User Name** : Your registered userId in SMS Gateway Center platform.
**Password** : Password for your account.
**Sender ID** : Enter your approved or desired sender id to send SMS from.

Lets save the form and enabled the SmsGatewayCenter provider to send SMS locally and international.

### SMS sending

To send SMS to records in the list view.

Click on the desired module. For example: contacts or organizations.
Select the desired records to which you want to send SMS. You can select records by clicking on the corresponding boxes that precede the records.
Now click on the Actions > **Send SMS** drop-down menu

Select the field where the mobile numbers of your contacts are stored.
Now type your text message content and click on Send.

## Issues

You can log your issues and we will be glad to fix if any.

Happy SMSing!
