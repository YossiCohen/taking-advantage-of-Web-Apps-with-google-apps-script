Taking advantage of Web Apps with Google Apps Script
=====

<a name="TOP"></a>
[![MIT License](http://img.shields.io/badge/license-MIT-blue.svg?style=flat)](LICENCE)

# Table of contents
- [Overview](#Overview)
- [Description](#Description)
- [Deploy Web Apps](#DeployWebApps)
- [5 situations for Web Apps](#SituationsforWebApps)
    - [How to access to Web Apps](#HowtoaccesstoWebApps)
        - [1. Owner accesses to Web Apps using browser](#HowtoaccesstoWebApps1)
        - [2. Client users access to Web Apps using browser](#HowtoaccesstoWebApps2)
        - [3. Owner accesses to Web Apps using Curl, Google Apps Script and so on which don't use browser](#HowtoaccesstoWebApps3)
        - [4. Client users access to Web Apps using Curl, Google Apps Script and so on which don't use browser](#HowtoaccesstoWebApps4)
    - [Required parameters for accessing to deployed Web Apps](#Requiredparameters)
    - [Authorization for scopes](#Authorizationforscopes)
    - [Access token for accessing to Web Apps](#AccesstokenforaccessingtoWebApps)
    - [Share project of Web Apps with client users](#Shareproject)
- [Limitation of simultaneous connection to Web Apps](#Limitationofsimultaneousconnection)
- [Error messages](#Errormessages)
- [Confidentiality of scripts for Web Apps](#Confidentialityofscripts)
- [Sample script of server side](#Samplescriptofserverside)
- [Sample scripts of client side](#Samplescriptofclientside)
- [Applications](#Applications)

<a name="Overview"></a>
# Overview
This is a report to take advantage of Web Apps with Google Apps Script (GAS).

<a name="Description"></a>
# Description
There is [Web Apps](https://developers.google.com/apps-script/guides/web) as one of applications using Google Apps Script (GAS). I sometimes use this Web Apps. But I have only a little the information for the specification of Web Apps. So in order to take more advantage of Web Apps, I investigated and summarized about this. The aim of this report is to become one of the basic information for creating various applications using Web Apps with GAS.

<a name="DeployWebApps"></a>
# Deploy Web Apps
When Web Apps is deployed, you can see the following window.

![](images/fig1.png)

For setting, generally, the following flow is used.

- On script editor
- Publish -> Deploy as web app...
    - Project version:
        - "New" and input the description.
    - Pattern 1: <u>Execute the app as:</u>
        - **Me**
            - <u>Who has access to the app:</u>
                - **Only myself**
                - **Anyone**
                - **Anyone, even anonymous**
    - Pattern 2: <u>Execute the app as:</u>
        - **User accessing the web app**
            - <u>Who has access to the app:</u>
                - **Only myself**
                - **Anyone**
    - Click Deploy.

There are **Me** and **User accessing the web app** for "Execute the app as:" and **Only myself**, **Anyone** and **Anyone, even anonymous** for "Who has access to the app:" as the options. When **Me** and **User accessing the web app** are selected for "Execute the app as:", each "Who has access to the app:" has 3 and 2 options, respectively. From above flow, It is found that there are 5 situations for deploying Web Apps. For each situation, there are 2 methods of GET and POST.

In this report, I would like to introduce about the specification for Web Apps deployed by 5 situations.

<a name="SituationsforWebApps"></a>
# 5 situations for Web Apps
1. **Situation 1**
    - "Execute the app as:" : **Me**
    - "Who has access to the app:": **Only myself**
1. **Situation 2**
    - "Execute the app as:" : **Me**
    - "Who has access to the app:": **Anyone**
1. **Situation 3**
    - "Execute the app as:" : **Me**
    - "Who has access to the app:": **Anyone, even anonymous**
1. **Situation 4**
    - "Execute the app as:" : **User accessing the web app**
    - "Who has access to the app:": **Only myself**
1. **Situation 5**
    - "Execute the app as:" : **User accessing the web app**
    - "Who has access to the app:": **Anyone**

<a name="HowtoaccesstoWebApps"></a>
## How to access to Web Apps
There are 5 situations for the deployed Web Apps, as mentioned above. And the methods for accessing to the deployed Web Apps are divided into 2 types. Those are the method which accesses using browser and the method which accesses using Curl, Google Apps Script and so on which don't use browser. Each method is used by the owner of Web Apps and the client users. The flow which summarized them is as follows.

<a name="HowtoaccesstoWebApps1"></a>
### 1. Owner accesses to Web Apps using browser
![](images/Browser_owner.png)

- For Situation 1, 4
    - Owner can access and run the script of Web Apps by the login to Google.
- For Situation 2, 5
    - Owner can access and run the script of Web Apps by the login to Google.
- For Situation 3
    - Owner can access and run the script of Web Apps without the login to Google.

<a name="HowtoaccesstoWebApps2"></a>
### 2. Client users access to Web Apps using browser
![](images/Browser_users.png)

- For Situation 1, 4
    - Client users cannot access and run the script of Web Apps.
- For Situation 2, 5
    - Client users can access and run the script of Web Apps by the login to Google.
- For Situation 3
    - Client users can access and run the script of Web Apps without the login to Google.

<a name="HowtoaccesstoWebApps3"></a>
### 3. Owner accesses to Web Apps using Curl, Google Apps Script and so on which don't use browser
![](images/Curl_owner.png)

- For Situation 1, 4
    - Owner can access and run the script of Web Apps by using the access token.
- For Situation 2, 5
    - Owner can access and run the script of Web Apps by using the access token.
- For Situation 3
    - If the script of Web Apps uses some scopes, owner has to authorize the scopes by own browser.
    - Owner can access and run the script of Web Apps without the login to Google.

<a name="HowtoaccesstoWebApps4"></a>
### 4. Client users access to Web Apps using Curl, Google Apps Script and so on which don't use browser
![](images/Curl_users.png)

- For Situation 1, 4
    - Client users cannot access and run the script of Web Apps.
- For Situation 2, 5
    - **At first, the project that Web Apps was deployed has to be shared with client users who use Web Apps.**
        - **I confirmed that from April 11, 2018, it is required to be shared the project to access to Web Apps. This might be due to the update of Google.**
    - If the script of Web Apps uses some scopes, client users have to authorize the scopes by own browser.
    - Client users can access and run the script of Web Apps without the login to Google.
- For Situation 3
    - If the script of Web Apps uses some scopes, client users have to authorize the scopes by own browser.
    - Client users can access and run the script of Web Apps without the login to Google.

<a name="Requiredparameters"></a>
## Required parameters for accessing to deployed Web Apps
The simple explanations for above 5 situations were summarized as the following table. Owner and client users mean the owner who deployed Web Apps and the users who use the deployed Web Apps, respectively.

| Situations | Script of Web Apps | Authorization for scopes | Owner | Client users | Share project |
|:---|:---|:---|:---|:---|:---|
| Situation 1 | Run as owner. | Only owner | Access with access token. | Cannot access. | No |
| Situation 2 | Run as owner. | Only owner | Access with access token. | Access with access token. | Yes |
| Situation 3 | Run as owner. | Only owner | Access without access token. | Access without access token. | No |
| Situation 4 | Run as each user. | Each user | Access with access token. | Cannot access. | No |
| Situation 5 | Run as each user. | Each user | Access with access token. | Access with access token. | Yes |

For example, at situation 5, the script of Web Apps is run as each user (owner and client users). The authorization for the scopes of scripts of Web Apps is required to run the scripts. This authorization has to be done by each user (owner and client users) using own browser. The access token is required for each user (owner and client users) to access to Web Apps. In order to access to Web Apps from client users, the project deployed Web Apps has to be shared with the users.

<a name="Authorizationforscopes"></a>
## Authorization for scopes
- When Web Apps is deployed as **``"Execute the app as:" : Me``** by owner, the authorization screen is automatically opened. When the owner authorizes it, owner and client users can run the scripts of Web Apps as the owner.

    - <u>**For example**</u>, in this situation, when ``Session.getEffectiveUser().getEmail()`` is run in this situation by the client users, the retrieved email is the owner's email. From this, it is found that the script is run as the owner.

    - <u>**For example**</u>, in this situation, when ``DriveApp.createFile(blob)`` is run at the script of Web Apps, the file is created in the owner's Google Drive.

- When Web Apps is deployed as **``"Execute the app as:" : User accessing the web app``** by owner, the authorization screen is **NOT** opened automatically. So before owner and client users access to Web Apps, they have to manually authorize to use the scopes by own browser.

    - If some scopes are used for the scripts of Web Apps, owner and client users have to authorize them only one time using own browser, before it accesses to Web Apps.

        - The URL for authorizing is ``https://script.google.com/macros/s/#####/exec`` which is the same to URL of Web Apps. When owner and client users access to the URL using own browser, the following authorization screen is displayed.

            - ![](images/fig2.png)

        - When you see above screen, please click "REVIEW PERMISSIONS". And select account and authorize to use the scopes. After owner and client users authorized the scopes, they can run the script of Web Apps.

    - <u>**For example**</u>, in this situation, when ``Session.getEffectiveUser().getEmail()`` is run in this situation by the client users, the retrieved email is the each user's email. From this, it is found that the script is run as each user.

    - <u>**For example**</u>, in this situation, when ``DriveApp.createFile(blob)`` is run at the script of Web Apps, the file is created in the each user's Google Drive.

<a name="AccesstokenforaccessingtoWebApps"></a>
## Access token for accessing to Web Apps
- When Web Apps is deployed as **``"Who has access to the app:": Only myself``** or **``"Who has access to the app:": Anyone``** by owner, the owner and client users have to access and run the script of Web Apps with own access token.

    - At least, one of scopes for using Drive API is required to be included in the access token. For example, those are
        - ``https://www.googleapis.com/auth/drive.readonly``
        - ``https://www.googleapis.com/auth/drive.files``
        - ``https://www.googleapis.com/auth/drive``

    - Even if the server script uses the scopes except for scripts to use Drive API, the scopes except for scopes for Drive API are not required to be included. Because the use of other scopes is authorized by the browser, before it accesses to Web Apps. This means that when it accesses to Web Apps by GET or POST request, only the scopes for using Drive API are required.

    - For example, when you want to access to Web Apps using GAS, even if you use ``{"Authorization": "Bearer " + ScriptApp.getOAuthToken()}`` to the headers, when the error of ``<TITLE>Unauthorized</TITLE>`` occurs, please confirm the scopes of the scripts at the script editor. If ``https://www.googleapis.com/auth/drive`` is not included in the scopes, please add it. In order to add it, for example, you can put ``// DriveApp.getFiles()`` in the scripts **as a comment**. By this, the script editor will automatically include ``https://www.googleapis.com/auth/drive``.

- If no scopes are used for the scripts of Web Apps, owner and client users can run the script of Web Apps without the authorization of scopes using own browser. But the access token is required to access to Web Apps, if it is **``"Who has access to the app:": Only myself``** or **``"Who has access to the app:": Anyone``**.

- Only when it is **``"Who has access to the app:": Anyone, even anonymous``**, owner and client users can access to Web Apps without the access token.

<a name="Shareproject"></a>
## Share project of Web Apps with client users
When client users access to Web Apps with Situation 2 and 5, the owner of Web Apps is required to share the project deployed Web Apps with the client users. If the project is not shared, the error message of ``<title>Google Drive - Access Denied</title>`` is returned. For example, if the project is the container-bound script with Spreadsheet, please share the spreadsheet with the client users.

For sharing the project, you can [manually share it](https://support.google.com/drive/answer/2494822) and use "VIEW" to the permissions.

Also you can share the project using GAS scripts. The sample GAS script for sharing the project with the client users is as follows. When you use this, please input email and fileId. For example, if the project is the container-bound script with Spreadsheet, the fileId is that of spreadsheet. If you want to send a notification email when the project is shared, please modify from ``sendNotificationEmail=false`` to ``sendNotificationEmail=true``.

~~~javascript
// DriveApp.getFiles() // This comment is used for including a scope of https://www.googleapis.com/auth/drive. Of course, you can use Manifests for the project.
var email = "### Client user's email address ###";
var fileId = "### fileId of project ###";

var url = "https://www.googleapis.com/drive/v3/files/" + fileId + "/permissions?sendNotificationEmail=false";
var params = {
    method: "post",
    headers: {"Authorization": "Bearer " + ScriptApp.getOAuthToken()},
    contentType: "application/json",
    payload: JSON.stringify({
        "role": "reader",
        "type": "user",
        "emailAddress": email,
    }),
    muteHttpExceptions: true,
};
var res = UrlFetchApp.fetch(url, params);
Logger.log(res);
~~~


<a name="Limitationofsimultaneousconnection"></a>
# Limitation of simultaneous connection to Web Apps
The limitation of simultaneous connection is also investigated. The fetchAll method added by recent Google's update was used for this investigation. [I have reported that the fetchAll method is worked by the asynchronous processing.](https://gist.github.com/tanaikech/c0f383034045ab63c19604139ecb0728) This can be used for measuring the limitation of simultaneous connection. From the result of measurement, it was found that the limitation of simultaneous connection to one Web Apps server is under 30. [This is the same with scripts.run method of Apps Script API.](https://github.com/tanaikech/RunAll)


<a name="Errormessages"></a>
# Error messages
When the error messages are returned from Web Apps, you can see the messages into the tag of ``<title>### Error message ###</title>`` including in HTML output. From the error messages, you can know the reason of the error using the following table. I think that there may be other errors. So if you found them, when you tell me them, I'm glad.

| Execute the app as | Who has access to the app | Access | Status code | Error messages | Reason |
|:---|:---|:---|:---|:---|:---|
| User accessing the web app | Only myself,<br>Anyone | Owner,<br>Users | 200 | Authorization needed | Scopes for scripts of Web Apps are not authorized. |
| User accessing the web app,<br>Me | Only myself,<br>Anyone | Owner,<br>Users | 200 | Meet Google Drive – One place for all your files | No access token. |
| For all settings | For all settings | Owner,<br>Users | 200 | Error | "Service invoked too many times in a short time: exec qps. Try Utilities.sleep(1000) between calls." is shown in Body. |
| User accessing the web app,<br>Me | Only myself,<br>Anyone | Owner,<br>Users | 401 | Unauthorized | Bad access token.<br>No required scopes. |
| User accessing the web app,<br>Me | Anyone | Users | 403 | Google Drive - Access Denied | Project of Web Apps is not shared with users. |
| User accessing the web app,<br>Me | Only myself | Users | 404 | Google Drive -- Page Not Found | Users cannot access. |

<a name="Confidentialityofscripts"></a>
# Confidentiality of scripts for Web Apps
- If you don't want to publish the scripts of Web Apps, you can achieve it using Situation 3.
    - In this case, anybody can access the Web Apps.

- If you want to make only the special users access to Web Apps, you can achieve it using Situation 2 and 5.
    - In this case, the scripts of Web Apps can be seen by the users, because the project of scripts has to be shared with the users.

<a name="Samplescriptofserverside"></a>
# Sample script of server side
The simple sample-script of server side (Web Apps) is as follows. When the client users access to this server, the server returns URL query and the request body which were sent by the client. And also a text file is created. In this script, the scope of ``https://www.googleapis.com/auth/drive`` is used for creating a text file. So when you use this script at Situation 4 and Situation 5 as mentioned above, at first, please authorize the scope by accessing to Web Apps using your browser. After the authorization was done, the text file is created by requesting the following scripts for the client side.

~~~javascript
function feedback(e, method) {
    var id = "";
    var user = Session.getEffectiveUser().getEmail();
    if (e.parameter.key1 == "value1") {
        id = DriveApp.createFile("sample.txt", "Created by " + user, MimeType.PLAIN_TEXT).getId();
    }
    return ContentService.createTextOutput(JSON.stringify({
        result: Object.keys(e.parameters).map(function(f) {return f + "," + e.parameters[f]}),
        method: method,
        createdFile: id ? id + " was created by " + user : "Not created.",
    })).setMimeType(ContentService.MimeType.JSON);
}

function doGet() {
    return feedback(e, "GET");
}

function doPost() {
    return feedback(e, "POST");
}
~~~

<a name="Samplescriptofclientside"></a>
# Sample scripts of client side
## Access with access token
### GET
#### Curl

~~~bash
curl -GL \
  -H "Authorization: Bearer ### your access token ###" \
  -d "key1=value1" \
  -d "key2=value2" \
  "https://script.google.com/macros/s/#####/exec?key3=value3"
~~~

#### Google Apps Script

~~~javascript
// DriveApp.getFiles() // This comment is used for including a scope of https://www.googleapis.com/auth/drive.
var params = {
    method: "GET",
    headers: {"Authorization": "Bearer " + ScriptApp.getOAuthToken()},
    muteHttpExceptions: true,
};
var url = "https://script.google.com/macros/s/#####/exec?key1=value1&key2=value2&key3=value3";
var res = UrlFetchApp.fetch(url, params);
Logger.log(res);
~~~

### POST
#### Curl

~~~bash
curl -L \
  -H "Authorization: Bearer ### your access token ###" \
  -d "key1=value1" \
  -d "key2=value2" \
  "https://script.google.com/macros/s/#####/exec?key3=value3"
~~~

#### Google Apps Script

~~~javascript
// DriveApp.getFiles() // This comment is used for including a scope of https://www.googleapis.com/auth/drive.
var params = {
    method: "POST",
    headers: {"Authorization": "Bearer " + ScriptApp.getOAuthToken()},
    payload: {key1: "value1", key2: "value2"},
    muteHttpExceptions: true,
};
var url = "https://script.google.com/macros/s/#####/exec?key3=value3";
var res = UrlFetchApp.fetch(url, params);
Logger.log(res);
~~~

## Access without access token
### GET
#### Curl

~~~bash
curl -GL \
  -d "key1=value1" \
  -d "key2=value2" \
  "https://script.google.com/macros/s/#####/exec?key3=value3"
~~~

#### Google Apps Script

~~~javascript
var params = {
    method: "GET",
    muteHttpExceptions: true,
};
var url = "https://script.google.com/macros/s/#####/exec?key1=value1&key2=value2&key3=value3";
var res = UrlFetchApp.fetch(url, params);
Logger.log(res);
~~~

### POST
#### Curl

~~~bash
curl -L \
  -d "key1=value1" \
  -d "key2=value2" \
  "https://script.google.com/macros/s/#####/exec?key3=value3"
~~~

#### Google Apps Script

~~~javascript
var params = {
    method: "POST",
    payload: {key1: "value1", key2: "value2"},
    muteHttpExceptions: true,
};
var url = "https://script.google.com/macros/s/#####/exec?key3=value3";
var res = UrlFetchApp.fetch(url, params);
Logger.log(res);
~~~

---

<a name="Applications"></a>
## Applications
Here, I would like to introduce the applications of Web Apps. If you can introduce your applications, please tell me. I would like to introduce them here.

### 1. Converting from a1Notation to GridRange
This is a sample API for [converting from a1Notation to GridRange](https://gist.github.com/tanaikech/95c7cd650837f33a564babcaf013cae0). The GridRange is often used at Sheets API. Although I have thought that a1Notation is easy to use, there are no methods for converting it. So I created this as an API. I would like to introduce this as a sample for this report.

The sample script is as follows. At first, please copy and paste it to new standalone project.

~~~javascript
function doGet(e) {
  var output = {};
  if (e.parameter.sheetid && e.parameter.a1notation) {
    output = {result: a1notation2gridrange1(e.parameter.sheetid, e.parameter.a1notation)};
  } else {
    output = {error: "Wrong parameters."};
  }
  return ContentService.createTextOutput(JSON.stringify(output));
}

// https://gist.github.com/tanaikech/95c7cd650837f33a564babcaf013cae0
function a1notation2gridrange1(sheetid, a1notation) {
  try {
    var data = a1notation.match(/(^.+)!(.+):(.+$)/);
    var ss = SpreadsheetApp.openById(sheetid).getSheetByName(data[1]);
    var range = ss.getRange(data[2] + ":" + data[3]);
    var gridRange = {
      sheetId: ss.getSheetId(),
      startRowIndex: range.getRow() - 1,
      endRowIndex: range.getRow() - 1 + range.getNumRows(),
      startColumnIndex: range.getColumn() - 1,
      endColumnIndex: range.getColumn() - 1 + range.getNumColumns(),
    };
    if (!data[2].match(/[0-9]/)) delete gridRange.startRowIndex;
    if (!data[3].match(/[0-9]/)) delete gridRange.endRowIndex;
    return gridRange;
  } catch (e) {
    return e;
  }
}
~~~

And then, please deploy this as Web Apps. The condition is as follows.

- "Execute the app as:" : **User accessing the web app**
- "Who has access to the app:": **Only myself**

By this condition, owner and each client user can use Spreadsheets on own Google Drive. Before you access to Web Apps, please don't forget to share the project to users. The usage is as follows. This is a curl sample. But of course, you can also use this API using Google Apps Script.

~~~bash
curl -GL \
  -H "Authorization: Bearer ### your access token ###" \
  -d "sheetid=### spreadsheet ID ###" \
  -d "a1notation=Sheet1%21A1%3aB2" \
  "https://script.google.com/macros/s/#####/exec"
~~~

When you use this, please do URL encode for the range.

### 2. URL to upload to Google Drive
[https://stackoverflow.com/questions/47833728/url-to-upload-to-google-drive](https://stackoverflow.com/questions/47833728/url-to-upload-to-google-drive)

### 3. How to get a data range of the Sheet
[https://stackoverflow.com/questions/48917258/how-to-get-a-data-range-of-the-sheet](https://stackoverflow.com/questions/48917258/how-to-get-a-data-range-of-the-sheet)


# References
- [Web Apps](https://developers.google.com/apps-script/guides/web)

---

<a name="Licence"></a>
# Licence
[MIT](LICENCE)

<a name="Author"></a>
# Author
[Tanaike](https://tanaikech.github.io/about/)

If you have any questions, feel free to tell me using e-mail tanaike@hotmail.com Also if you found new information related to this report and tell me them, I am glad. I would like to update this report.

<a name="Update_History"></a>
# Update History
* v1.0.0 (April 26, 2018)

    Initial release.

[TOP](#TOP)