# xM Labs Step Template
Template for contributing Flow Designer steps to the [Flow Steps](https://github.com/xmatters/xMatters-Labs-Flow-Steps) repo


---------

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</kbd>

---------


# Application Name Goes Here
One or two liner about the product here. 

## Action Name 1
The "Create App Record" step creates a record in the APPLICATION_NAME and such and such. 

### Settings
Values for the settings tab. A screen shot is also acceptable. If you have a custom icon, please upload it into the `/media` folder and reference with `<kbd> <img src="/media/hat.png"></kbd>`. See below for the format of a table in markdown. 

| Field | Value |
| ----- | ----- |
| Name | Create a Hat |
| Description | Sends a create a hat request to the hat factory.  |
| Icon | <kbd> <img src="/media/hat.png"></kbd> |
| Include Endpoint | Yes |
| Endpoint Type | Basic Auth |
| Endpoint Label | Hat Factory |



### Inputs
Inputs should be in the form of tables. The syntax looks like this in the markdown. The "content cells" do not have to line up, just make sure you have the right number of columns in each row. Required inputs must be at the top and please for the love of Pete, add some helpful text as to what the input is for or what it does. If the step only allows certain values, make sure to list them!

```
| Name  | Required? | Min | Max | Help Text | Default Value | Multiline |
| ----- | ----------| --- | --- | --------- | ------------- | --------- |
| Name  | Yes | 0 | 2000 | This is the color of the hat to create | Blue | No |
| Color | No | 0 | 2000 | The major color of the hat. Possible values are Red, White, Blue | Blue | No |
```

| Name  | Required? | Min | Max | Help Text | Default Value | Multiline |
| ----- | ----------| --- | --- | --------- | ------------- | --------- |
| Name  | Yes | 0 | 2000 | This is the color of the hat to create | Blue | No |
| Color | No | 0 | 2000 | The major color of the hat. Possible values are Red, White, Blue | Blue | No |


### Outputs

| Name |
| ---- |
| Hat |
| Link |

### Script

```javascript
// Retrieve the name and color values
var name = input['Name'];
var color = input['Color'];

// Make the request
var req = http.request({
   "endpoint": "Hat Factory",
   "path": "/hat",
   "method": "POST",
   "headers": {
      "Content-Type": "application/json"
   }
});

var hatPayload = {
   "name": name,
   "color": color
};

var resp = req.write( hatPayload );

```


## Action Name 2
The "Update App Record" step creates a record in the APPLICATION_NAME and such and such. 

### Settings

**Name**: Update a Hat
**Description**: Sends an update a hat request to the hat factory. 
**Icon**: <kbd> <img src="/media/hat.png"></kbd>
**Include Endpoint**: Yes
**Endpoint Type**: Basic Auth
**Endpoint Label**: Hat Factory

### Inputs

| Name  | Required? | Min | Max | Help Text | Default Value | Multiline |
| ----- | ----------| --- | --- | --------- | ------------- | --------- |
| Name  | Yes | 0 | 2000 | This is the color of the hat to create | Blue | No |
| Color | No | 0 | 2000 | The major color of the hat. Possible values are Red, White, Blue | Blue | No |


### Outputs

| Name |
| ---- |
| Hat |
| Link |

### Script

```javascript
// Retrieve the name and color values
var name = input['Name'];
var color = input['Color'];

// Make the request
var req = http.request({
   "endpoint": "Hat Factory",
   "path": "/hat",
   "method": "POST",
   "headers": {
      "Content-Type": "application/json"
   }
});

var hatPayload = {
   "name": name,
   "color": color
};

var resp = req.write( hatPayload );

```
