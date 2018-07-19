# saymyname
'use strict';
const Alexa = require('alexa-sdk');
const APP_ID = 'amzn1.ask.skill.bb43d508-732b-4801-95fd-3a56fa2a67ce';
const SKILL_NAME = 'say my name';
const HELP_MESSAGE = 'What can I help you with?';
const STOP_MESSAGE = 'Goodbye!';

function buildhandlers(event) {
    var handlers = {
        'LaunchRequest': function () {
            this.emit(':ask','Hi, Welcome, How can i help you?');
        },
        'sayMyNameIntent': function () {
            if(this.attributes['userName']){
                this.emit(':tell','Hello! '+ this.attributes['userName'] + ' Nice to meet you');
            }
            else{
             this.emit(':ask','What is your name?');   
            }
        },
        'myNameIsIntent': function(){
            const userName = event.request.intent.slots.name.value;
            this.attributes['userName'] = userName;
            this.emit(':tell','Hello! '+ userName + ' Nice to meet you');
        },
        'AMAZON.HelpIntent': function () {
            const speechOutput = HELP_MESSAGE;
            this.response.speak(speechOutput);
            this.emit(':responseReady');
        },
        'AMAZON.CancelIntent': function () {
            this.response.speak(STOP_MESSAGE);
            this.emit(':responseReady');
        },
        'AMAZON.StopIntent': function () {
            this.response.speak(STOP_MESSAGE);
            this.emit(':responseReady');
        },
        
};
return handlers;

}


exports.handler = function (event, context) {
    const alexa = Alexa.handler(event, context);
    alexa.APP_ID = APP_ID;
    alexa.dynamoDBTableName ='saymyname';
    alexa.registerHandlers(buildhandlers(event));
    alexa.execute();
};

