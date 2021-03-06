### OBJECTIVE

This Facebook connector extends from the [Chatbot API Connector](https://github.com/inbenta-integrations/chatbot_api_connector) library. This library includes a Facebook API Client in order to send messages to Facebook users through our Facebook Page. It translates Facebook Messenger's messages into the Inbenta Chatbot API format and vice versa. Also, it implements some methods from the base HyperChat client in order to communicate with Facebook Messenger when the user is chatting.

### FUNCTIONALITIES
This connector inherits the functionalities from the `ChatbotConnector` library. Currently, the features provided by this application are:

* Simple answers
* Multiple options with a limit of 3 elements (Facebook only allows a maximum number of three options)
* Polar questions
* Chained answers
* Content ratings (yes/no + comment)
* Escalate to HyperChat after a number of no-results answers
* Escalate to HyperChat when matching with an 'Escalation FAQ'
* Send information to webhook through forms

### HOW TO CUSTOMIZE

**Custom Behaviors**

If you need to customize the bot flow, you need to modify the class `FacebookConnector.php`. This class extends from the ChatbotConnector and here you can override all the parent methods.


### STRUCTURE

The `FacebookConnector` folder has some classes needed to use the ChatbotConnector with Facebook. These classes are used in the FacebookConnector constructor in order to provide the application with the components needed to send information to Facebook and to parse messages between Facebook, ChatbotAPI and HyperChat.

**External API folder**

Inside this folder there is the Facebook API client which allows the bot to send messages and handle authorization events.


**External Digester folder**

This folder contains the class FacebookDigester. This class is a kind of "translator" between the Chatbot API and Facebook. Mainly, the work performed by this class is to convert a message from the Chatbot API into a message accepted by the Facebook Messenger API. It also does the inverse work, translating messages from Facebook into the format required by the Chatbot API.


**HyperChat API**

The class `FacebookHyperChatClient` instantiates a Facebook client from the `external_id` parameter provided by HyperChat. This parameter is generated by the external client and passed to HyperChat when the chat is created. This parameter allows us to instantiate a new Facebook client from Facebook user_id that is extracted from the external_id:
```php
    //Instances an external client
    protected function instanceExternalClient($externalId, $appConf){
        $facebookId = FacebookAPIClient::getIdFromExternalId($externalId);
        $externalClient = new FacebookAPIClient($appConf->get('fb.page_access_token'));
        $externalClient->setSenderFromId( $facebookId );
        return $externalClient;
    }
```

This class extends from the class `HyperChatClient` that extends from the default HyperChatClient provided by the product team, and all the parent methods can be overwritten.
