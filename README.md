# My Weather App using DraftBit

### Setup
1. Login into your DraftBit account and select 'Create App' from the top right hand corner. Provide the name of the app & short description in the pop-up that showsup and click 'Continue'. In the nest follow-up pop-up screen, choose 'Start with blank app'. Wait for the app to finish building and then click on 'Start Building'.
 
### Building the UI
1. This app is going to have only one screen. For your information, if you wish to add more screens, click on the "+" button in the top left pane which has a "Screens" section.
2. By default, we are provided with a "Blank" screen. Double click on "Blank" screen and change its name. This is what appears in the header bar of the app.
3. Now lets configure the header bar style. Click on the "Navigate" tab in the center top of the screen. 
- The left pane will change to "Navigation". Click on the screen below the Root navigator.
- On the right pane, select the "Background" property of the Header and change to desired color.
- Similarly, select the desired Text Color of the header text.
4. Now switch back to the "Design" tab from the top center.
5. Now we will build the UI for the app. 
- Click on the "+" button in the "Components" tab, search for 'View' and add it to wrap all the controls.
- Now click on the added 'View' to select it and change it style. From the 'Layout' section in the right pane, set the 'Align' property to 'Center' and 'Justify' property to 'Flex start'. 
- Again click on "+" in the Components tab.
- Search for 'Text' and add two 'Text' controls. 
- Next search for 'View' and add a new View control.
- Select the first text control to modify its style and value. From the 'Layout' section in the right pane, set the 'Align' property to 'Center'. From the 'Typography' section, set the Font to "Acme" and size to 24 pt.
- From the thrid tab in the right pane, add 'Welcome to My Weather App!' in the 'Input Text' section.
- Now select the second Text control to modify its style and value. From the 'Layout' section in the right pane, set the 'Align' property to 'Center'. From the 'Typography' section, set the size to 16 pt.
- From the thrid tab in the right pane, add 'Click below to know your current weather!' in the 'Input Text' section.
- Now select the 'View' control to modify its style. From the 'Layout' section in the right pane, set the 'Align' property to 'Center' and 'Justify' property to 'Center'. 
- Click on the "+" button in the Components pane and search for 'Button' and add it.
- Next search for 'Text' and add it to the same view.

-  Click on the 'Button' to select it and change it's style. In the right pane, change the Background to a custom colour of your choice. Change the border radius to 4. Now click on the third tab and change the text to 'Get Weather'.
-  Click on the 'Text' to selet it and change it's style. In the right pane, under 'Layout' set the Align property to 'Center' and 'Typography Align' to 'Left'.
  
### Implementing functionality

1. Let's first indicate that the app will be using the Location services from the device. We can do this by going to Project settings, by click on the Gear icon in the top menu bar.
2. In the pop-up that shows up, go to 'Advanced Settings' and under 'IOS' -> Permissions, tick mark the 'Location Access' and provide the description of why you need the location access in the text below. Next under 'Android' -> Permissions, tick mark the 'Coarse Location' and 'Fine Location' permissions.
3. Finally click 'Save'
4. Next we need to import the `expo-location` package into our app. This package will give our app access to the user's device geolocation information.
5. Open the Custom Code panel using the button in the top menu bar
6. Click the Packages tab on the left. Click the '+' to add a row. Enter `expo-location` for the Package Name. Enter `latest` for the Version. Click the Save button.
7. To make the Location methods accessible to your functions, From the Custom Code panel, click the Components tab on the left and Paste the following line of code below the import statements and Click the Save button: 

```
export { requestForegroundPermissionsAsync, getCurrentPositionAsync } from 'expo-location';
```
8. Now let's Create a function to request the user's location.
9. From the Custom Code panel, click the Functions tab on the left. For the Type choose Custom Function. Give it a descriptive name, like 'fetchWeather'. Set the Return type to Object. Activate the Is function asynchronous switch. Paste the following code into the editor:

```
let { status } = await CustomCode.requestForegroundPermissionsAsync();

if (status !== 'granted') {
    alert('Access to your location is needed to determine the current weather!');
    return null;
}

let location = await CustomCode.getCurrentPositionAsync({});

let response = await fetch(
      `http://api.openweathermap.org/data/2.5/weather?lat=${location.coords.latitude}&lon=${location.coords.longitude}&APPID=17ca349a7b6e8ce7d1c45f5dde0d6c1f&units=metric`
    );

let weatherResponse = await response.json();

// alert(JSON.stringify(weatherResponse));

return weatherResponse;
```
Note that we registered on the Open weather website to get the API Key.

10. Next, let's setup some variables so that they can be referenced from the UI. Select 'Variables' from the top in the menu bar. In the pop-up that shows-up, select 'Screen Variables' and create the below ones:
- countryName of type 'String'
- currentWeather of type 'Object'
- humidity of type 'Number'
- latitude of type 'Number'
- longitude of type 'Number'
- placeName of type 'String'
- pressure of type 'Number'
- temperature of type 'Number'
- windSpeed of type 'Number'

11. Now click on the 'Button' in the Comoponents tree to select it and to the fourth tab of the properties pane on the right. Click on the "+" icon next to 'Actions' and select 'Run a Custom Function' and then select the custom function created in the previous step from the dropdown. Enter `currentWeather` as the Result Name. This is the varialbe we'll use to extract the data from the API response.
- Next, let's extract data from the response and save it into variables.
- Click on the '+' in the 'Actions' pane and select 'Extract Key'. Choose Input as 'currentWeather', Path as `.coord.lat` and provide a Result Name as 'latitude' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Extract Key'. Choose Input as 'currentWeather', Path as `.coord.lon` and provide a Result Name as 'longitude' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Extract Key'. Choose Input as 'currentWeather', Path as `.sys.country` and provide a Result Name as 'countryName' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Extract Key'. Choose Input as 'currentWeather', Path as `.name` and provide a Result Name as 'placeName' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Extract Key'. Choose Input as 'currentWeather', Path as `.main.temp` and provide a Result Name as 'temperature' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Extract Key'. Choose Input as 'currentWeather', Path as `.main.pressure` and provide a Result Name as 'pressure' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Extract Key'. Choose Input as 'currentWeather', Path as `.wind.speed` and provide a Result Name as 'windSpeed' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Extract Key'. Choose Input as 'currentWeather', Path as `.main.humidity` and provide a Result Name as 'humidity' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Set Variable'. Choose Variable as 'longitude' and New Value as 'longitude' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Set Variable'. Choose Variable as 'latitude' and New Value as 'latitude' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Set Variable'. Choose Variable as 'countryName' and New Value as 'countryName' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Set Variable'. Choose Variable as 'placeName' and New Value as 'placeName' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Set Variable'. Choose Variable as 'temperature' and New Value as 'temperature' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Set Variable'. Choose Variable as 'pressure' and New Value as 'pressure' and the click on 'X'.
- Again, click on '+' in the Actions pane and select 'Set Variable'. Choose Variable as 'windSpeed' and New Value as 'windSpeed' and the click on 'X'.

12. Next click on the bottom most text (below the button) and select the third tab in the Properties pane from the right. Set the value of Input Text as below:

```
Latitude: {{ latitude }} 
Longitude: {{ longitude }} 
Location: {{placeName}} {{countryName}}
Temperature: {{temperature}}
Pressure: {{pressure}}
Wind Speed: {{windSpeed}}
Humidity: {{humidity}}
```

13. Inside the Variables pane, set the values of the variables to corresponding variables craeted in the previous step. For instance, set `latitude` to `latitude` and so on.
14. Now we are ready to test our app. Click on the 'Apple' icon to test on iOS and 'Android' icon to test on Android platform. Clicking on the 'Get Weather' button should ask for the permission and then fetch the weather data once the permission is granted.


