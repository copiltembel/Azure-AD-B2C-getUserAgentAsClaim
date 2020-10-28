# What is this?

This is an example of how to gather (user) attributes that are NOT (currently - Oct. 2020) available through the Azure ADB2C (See https://docs.microsoft.com/en-us/azure/active-directory-b2c/claim-resolver-overview).
Here are some examples of such attributes:

- Client's UserAgent (ex. https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/35235046-send-a-user-agent-when-an-azure-ad-b2c-custom-poli)
- Client's Timezone
- the sky is the limit.

# How does it work?

The idea is to combine [Azure ADB2C's custom attributes](https://docs.microsoft.com/en-us/azure/active-directory-b2c/user-flow-custom-attributes) with [AADB2C js scripts](https://docs.microsoft.com/en-us/azure/active-directory-b2c/javascript-samples) to gather and send this data.

![This is what I mean](Capture.PNG?raw=true)

# Implementation
1. Create an Azure ADB2C custom attribute (I called mine 'extension_CustomInfo')
2. Add it to your custom flow (i.e. the xml file that defines your custom policy). For example, I added the following to my SignUp flow, but you can added to any flow you want:

```
[...]
  <BuildingBlocks>
    <ClaimsSchema>
    [...]
      </ClaimType>
        <ClaimType Id="extension_CustomInfo">
        <DisplayName>CustomInfo</DisplayName>
        <DataType>string</DataType>
        <UserHelpText>Some custom info</UserHelpText>
        <UserInputType>TextBox</UserInputType>
      </ClaimType>
    </ClaimsSchema>
  </BuildingBlocks>
[...]
```

3. Customize your [Azure ADB2C layout](https://docs.microsoft.com/en-us/azure/active-directory-b2c/custom-policy-ui-customization)
4. Add js code that fills in and hides the custom attribute created earlier (since this is filled in automatically, no need to display it to the user). For example, I added the following to my selfAsserted.html:

```
    <script>
    
        function hideCustomInfoInput(){
          var customInfo = document.querySelector('#extension_CustomInfo');
          console.log('This is customInfo', customInfo)
          if (customInfo)
            $("#extension_CustomInfo").hide();
        }
        
        function fillInUserAgent() {
            var customInfo = document.querySelector('#extension_CustomInfo');
            if (customInfo) {
                $("#extension_CustomInfo").val(navigator.userAgent);
            }
    
        }
    
        hideCustomInfoInput();
        fillInUserAgent();
    </script>
```

5. That's it! You will receive the filled in extension_CustomInfo attribute alongside the other claims.

# Limitations
There's a 256 character limit on the Azure ADB2C attributes. If you need more, why not just add more hidden attributes like the one above?

# "Hey, this is hacky!" :rage:
Yes ... and? :nail_care:


