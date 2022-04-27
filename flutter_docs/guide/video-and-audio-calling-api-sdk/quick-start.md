---
title: Quick Start with Flutter
hide_title: false
hide_table_of_contents: false
description: Video SDK enables the opportunity to integrate native IOS, Android & Web SDKs to add live video & audio conferencing to your applications.
sidebar_label: Start a Voice / Video Call
pagination_label: Quick Start with Flutter
keywords:
  - audio calling
  - video calling
  - real-time communication
  - collabration
image: img/videosdklive-thumbnail.jpg
sidebar_position: 1
slug: quick-start
---

# Quick Start

VideoSDK enables the opportunity to integrate video & audio calling to Web, Android, IOS applications. it provides Programmable SDKs and REST APIs to build scalable video conferencing applications.

This guide will get you running with the VideoSDK video & audio calling in minutes.

## Prerequisites

Before proceeding, ensure that your development environment meets the following requirements:

- Flutter SDK installed

:::important

One should have a videoSDK account to generate token.
Visit videoSDK **[dashboard](https://app.videosdk.live/api-keys)** to generate token

:::

## Project Setup

Follow the steps to create the environment necessary to add video calls into your app.

1. Create a new Flutter project.

```js
$ flutter create videosdk_flutter_quickstart
```

2. Directory Structure

```js title="Directory Structure"
root-Folder Name
   ├── ...
   ├── lib
   │    ├── join_screen.dart
        ├── main.dart
        ├── meeting_screen.dart
        ├── participant_grid_view.dart
        ├── participant_tile.dart

```

3. Once the Flutter project is created, run the following command to add Flutter VideoSDK to the project.

```js
$ flutter pub add videosdk

//run this command to add http library to perform network call to generate meetingId
$ flutter pub add http
```

4. Update the `AndroidManifest.xml` for the permissions we will be using to implement the audio and video features. You can find the `AndroidManifest.xml` file at `<project root>/android/app/src/main/AndroidManifest.xml`

```js title="AndroidManifest.xml"
<uses-feature android:name="android.hardware.camera" />
<uses-feature android:name="android.hardware.camera.autofocus" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
```

5. Also you will need to set your build settings to Java 8 because the official WebRTC jar now uses static methods in `EglBase` interface. Just add this to your app-level `build.gradle`

```js
android {
    //...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

6. If necessary, in the same `build.gradle` you will need to increase `minSdkVersion` of `defaultConfig` up to `23` (currently default Flutter generator set it to `16`).

If necessary, in the same `build.gradle` you will need to increase `compileSdkVersion` and `targetSdkVersion` up to `31` (currently default Flutter generator set it to `30`).

7. Let's complete the iOS Setup for the app.

Add the following entries which allow your app to access the camera and microphone to your Info.plist file, located in `<project root>`/ios/Runner/Info.plist:

```xml
<key>NSCameraUsageDescription</key>
<string>$(PRODUCT_NAME) Camera Usage!</string>
<key>NSMicrophoneUsageDescription</key>
<string>$(PRODUCT_NAME) Microphone Usage!</string>
```

## Start Writing Your Code

### Step 1 : Creating the Joining Screen

The Joining screen will consist of:

- Create Button - This button will create a new meeting for you.
- TextField for Meeting ID - This text field will contain the meeting ID you want to join.
- Join Button - This button will join the meeting which you provided.

1. Create a new dart file `join_screen.dart` which will contain our Stateful Widget named `JoinScreen`.

Replace the `_token` with the sample token you generated from the [VideoSDK Dashboard](https://app.videosdk.live/api-keys).

```js title="join_screen.dart"
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'package:videosdk_flutter_quickstart/meeting_screen.dart';

class JoinScreen extends StatefulWidget {
  const JoinScreen({Key? key}) : super(key: key);

  @override
  _JoinScreenState createState() => _JoinScreenState();
}

class _JoinScreenState extends State<JoinScreen> {
  //Repalce the token with the sample token you generated from the VideoSDK Dashboard
  String _token ="";

  String _meetingID = "";

  @override
  Widget build(BuildContext context) {
    final ButtonStyle _buttonStyle = TextButton.styleFrom(
      primary: Colors.white,
      backgroundColor: Theme.of(context).primaryColor,
      textStyle: const TextStyle(
        fontWeight: FontWeight.bold,
      ),
    );
    return Scaffold(
      appBar: AppBar(
        title: const Text("VideoSDK RTC"),
      ),
      backgroundColor: Theme.of(context).backgroundColor,
      body: SafeArea(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [

          ],
        ),
      ),
    );
  }

}
```

2. Update the `JoinScreen` with two buttons and a text field.

```js title="join_screen.dart"
class _JoinScreenState extends State<JoinScreen> {
  //Repalce the token with the sample token you generated from the VideoSDK Dashboard
  String _token ="";

  String _meetingID = "";

  @override
  Widget build(BuildContext context) {
    final ButtonStyle _buttonStyle = TextButton.styleFrom(
      primary: Colors.white,
      backgroundColor: Theme.of(context).primaryColor,
      textStyle: const TextStyle(
        fontWeight: FontWeight.bold,
      ),
    );
    return Scaffold(
      appBar: AppBar(
        title: const Text("VideoSDK RTC"),
      ),
      backgroundColor: Theme.of(context).backgroundColor,
      body: SafeArea(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            //Button to Create Meeting
            TextButton(
              style: _buttonStyle,
              onPressed: () async {
                //When clicked
                //Generate a meetingId
                _meetingID = await createMeeting();

                //Navigatet to MeetingScreen
                navigateToMeetingScreen();
              },
              child: const Text("CREATE MEETING"),
            ),
            SizedBox(height: 20),
            const Text(
              "OR",
              style: TextStyle(
                fontWeight: FontWeight.bold,
                color: Colors.white,
                fontSize: 24,
              ),
            ),
            SizedBox(height: 20),
            //Textfield for entering meetingId
            Padding(
              padding: const EdgeInsets.symmetric(horizontal: 32.0),
              child: TextField(
                onChanged: (meetingID) => _meetingID = meetingID,
                decoration: InputDecoration(
                  border: const OutlineInputBorder(),
                  fillColor: Theme.of(context).primaryColor,
                  labelText: "Enter Meeting ID",
                  hintText: "Meeting ID",
                  prefixIcon: const Icon(
                    Icons.keyboard,
                    color: Colors.white,
                  ),
                ),
              ),
            ),
            SizedBox(height: 20),
            //Button to join the meeting
            TextButton(
              onPressed: () async {
                //Navigate to MeetingScreen
                navigateToMeetingScreen();
              },
              style: _buttonStyle,
              child: const Text("JOIN MEETING"),
            )
          ],
        ),
      ),
    );
  }

  //This is method is called to navigate to MeetingScreen.
  //It passes the token and meetingId to MeetingScreen as parameters
  void navigateToMeetingScreen(){
    Navigator.push(
      context,
      MaterialPageRoute(
        //MeetingScreen is created in upcomming steps
        builder: (context) => MeetingScreen(
          token: _token,
          meetingId: _meetingID,
          displayName: "John Doe",
        ),
      ),
    );
  }

}
```

3. Add the `createMeeting()` in the `JoinScreen` which will generate a new meeting id.

```js title="join_screen.dart"
class _JoinScreenState extends State<JoinScreen> {

  //...other variables

  //...build method

  Future<String> createMeeting() async {
    final Uri getMeetingIdUrl =
        Uri.parse('https://api.videosdk.live/v1/meetings');
    final http.Response meetingIdResponse =
        await http.post(getMeetingIdUrl, headers: {
      "Authorization": _token,
    });

    final meetingId = json.decode(meetingIdResponse.body)['meetingId'];
    return meetingId;
  }
}
```

4. Make the `JoinScreen` as your home in the `main.dart` as shown below.

```js title="main.dart"
void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: JoinScreen(), //change to this
    );
  }
}
```

### Step 2 : Creating the Meeting Screen

1. Create a new `meeting_screen.dart` which will have a Stateful widget named `MeetingScreen`.

`MeetingScreen` accepts the following parameter:

- `meetingId`: This will be the meeting Id we will joining
- `token`: Auth Token to configure the Meeting
- `displayName`: Name with which the participant will be joined
- `micEnabled` : (Optional) If true, the mic will be on when you join the meeting else it will be off.
- `webcamEnabled`: (Optional) If true, the webcam will be on when you join the meeting else it will be off.

```js title="meeting_screen.dart"
import 'package:flutter/material.dart';
import 'package:videosdk/rtc.dart';
import 'package:videosdk_flutter_quickstart/join_screen.dart';
import 'package:videosdk_flutter_quickstart/participant_grid_view.dart';

class MeetingScreen extends StatefulWidget {

  //add the following parameters for your MeetingScreen
  final String meetingId, token, displayName;
  final bool micEnabled, webcamEnabled;
  const MeetingScreen({
    Key? key,
    required this.meetingId,
    required this.token,
    required this.displayName,
    this.micEnabled = true,
    this.webcamEnabled = true
  }) : super(key: key);

  @override
  _MeetingScreenState createState() => _MeetingScreenState();
}

class _MeetingScreenState extends State<MeetingScreen> {

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

2. Now we will update `_MeetingScreenState` to use the `MeetingBuilder` to create our meeting.

We will pass the required parameters to the MeetingBuilder.

```js title="meeting_screen.dart"
class _MeetingScreenState extends State<MeetingScreen> {

  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: _onWillPopScope,
      //MeetingBuilder is a class of @videosdk/rtc.dart
      child: MeetingBuilder(
        meetingId: widget.meetingId,
        displayName: widget.displayName,
        token: widget.token,
        micEnabled: widget.micEnabled,
        webcamEnabled: widget.webcamEnabled,
        notification: const NotificationInfo(
          title: "Video SDK",
          message: "Video SDK is sharing screen in the meeting",
          icon: "notification_share", // drawable icon name
        ),
        builder: (_meeting) {

        },
      ),
    );
  }
}
```

3. Now we will add `meeting`, `videoStream`, `audioStream` which will store the meeting and the streams for local participant.

```js title="meeting_screen.dart"
class _MeetingScreenState extends State<MeetingScreen> {

  Meeting? meeting;

  Stream? videoStream;
  Stream? audioStream;

  //...build

}
```

4. Now we will update the `builder` to generate a meeting view.

- Adding the Event listeners for the meeting and setting the state of our local `meeting`

```js title="meeting_screen.dart"
class _MeetingScreenState extends State<MeetingScreen> {

  //...state declartations

  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: _onWillPopScope,
      child: MeetingBuilder(
        //meetingConfig

        builder: (_meeting) {
          // Called when joined in meeting
          _meeting.on(
            Events.meetingJoined,
            () {
              setState(() {
                meeting = _meeting;
              });

              // Setting meeting event listeners
              setMeetingListeners(_meeting);
            },
          );
        }
      ),
    );
  }

  void setMeetingListeners(Meeting meeting) {
    // Called when meeting is ended
    meeting.on(Events.meetingLeft, () {
      Navigator.pushAndRemoveUntil(
          context,
          MaterialPageRoute(builder: (context) => const JoinScreen()),
          (route) => false);
    });

    // Called when stream is enabled
    meeting.localParticipant.on(Events.streamEnabled, (Stream _stream) {
      if (_stream.kind == 'video') {
        setState(() {
          videoStream = _stream;
        });
      } else if (_stream.kind == 'audio') {
        setState(() {
          audioStream = _stream;
        });
      }
    });

    // Called when stream is disabled
    meeting.localParticipant.on(Events.streamDisabled, (Stream _stream) {
      if (_stream.kind == 'video' && videoStream?.id == _stream.id) {
        setState(() {
          videoStream = null;
        });
      } else if (_stream.kind == 'audio' && audioStream?.id == _stream.id) {
        setState(() {
          audioStream = null;
        });
      }
    });
  }

}
```

- We will be showing a Waiting to join screen until the meeting is joined and then show the meeting view.

```js title="meeting_screen.dart"
class _MeetingScreenState extends State<MeetingScreen> {

  //...state declartations

  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: _onWillPopScope,
      child: MeetingBuilder(
        //meetingConfig

        builder: (_meeting) {
          //_meeting listener

          // Showing waiting screen
          if (meeting == null) {
            return Scaffold(
              body: Center(
                child: Column(
                  mainAxisSize: MainAxisSize.min,
                  children: [
                    const CircularProgressIndicator(),
                    SizedBox(height: 20),
                    const Text("waiting to join meeting"),
                  ],
                ),
              ),
            );
          }

          //Meeting View
          return Scaffold(
            backgroundColor: Theme.of(context).backgroundColor.withOpacity(0.8),
            appBar: AppBar(
              title: Text(widget.meetingId),
            ),
            body: Column(
              children: [

              ],
            ),
          );
        }
      ),
    );
  }

}
```

- Inside our meeting view we will add a `ParticipantGrid` and three action buttons.

```js title="meeting_screen.dart"
class _MeetingScreenState extends State<MeetingScreen> {

  //...state declartations

  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: _onWillPopScope,
      child: MeetingBuilder(
        //...meetingConfig

        builder: (_meeting) {
          //... _meeting listener

          //...Waiting Screen UI

          //Meeting View
          return Scaffold(
            backgroundColor: Theme.of(context).backgroundColor.withOpacity(0.8),
            appBar: AppBar(
              title: Text(widget.meetingId),
            ),
            body: Column(
              children: [
                Expanded(
                  //ParticipantGridView will be created in further steps!
                  child: ParticipantGridView(meeting: meeting!),
                ),
                Row(
                  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                  children: [
                    ElevatedButton(
                      onPressed: () => {
                        if (audioStream != null)
                          {_meeting.muteMic()}
                        else
                          {_meeting.unmuteMic()}
                      },
                      child: Text("Mic"),
                    ),
                    ElevatedButton(
                      onPressed: () => {
                        if (videoStream != null)
                          {_meeting.disableWebcam()}
                        else
                          {_meeting.enableWebcam()}
                      },
                      child: Text("Webcam"),
                    ),
                    ElevatedButton(
                      onPressed: () => {_meeting.leave()},
                      child: Text("Leave"),
                    ),
                  ],
                )
              ],
            ),
          );
        }
      ),
    );
  }

}
```

- Add the `_onWillPopScope()` which will handle the meeting leave on back button click.

```js title="meeting_screen.dart"
class _MeetingScreenState extends State<MeetingScreen> {

  //...other declarations

  //...build method

  //... setMeetingListeners

  Future<bool> _onWillPopScope() async {
    meeting?.leave();
    return true;
  }
}
```

5. Next we will be creating the `ParticipantGridView` which will be used to show the participant's view.

`ParticipantGridView` maps each participant with a `ParticipantTile`.

It updates the participants list whenever someone leaves or joins the meeting using the `participantJoined` and `participantLeft` event listeners on the `meeting`.

```js title="participant_grid_view.dart"
import 'package:flutter/material.dart';
import 'package:videosdk/rtc.dart';

import 'participant_tile.dart';

class ParticipantGridView extends StatefulWidget {
  final Meeting meeting;
  const ParticipantGridView({
    Key? key,
    required this.meeting,
  }) : super(key: key);

  @override
  State<ParticipantGridView> createState() => _ParticipantGridViewState();
}

class _ParticipantGridViewState extends State<ParticipantGridView> {
  Participant? localParticipant;
  Map<String, Participant> participants = {};

  @override
  void initState() {
    // Initialize participants
    localParticipant = widget.meeting.localParticipant;
    participants = widget.meeting.participants;

    // Setting meeting event listeners
    setMeetingListeners(widget.meeting);
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return GridView.count(
      crossAxisCount: 2,
      children: [
        //This Participant Tile will hold local participants view
        ParticipantTile(
          participant: localParticipant!,
          isLocalParticipant: true,
        ),
        //This will map all other participants
        ...participants.values
            .map((participant) => ParticipantTile(participant: participant))
            .toList()
      ],
    );
  }

  void setMeetingListeners(Meeting _meeting) {
    // Called when participant joined meeting
    _meeting.on(
      Events.participantJoined,
      (Participant participant) {
        final newParticipants = participants;
        newParticipants[participant.id] = participant;
        setState(() {
          participants = newParticipants;
        });
      },
    );

    // Called when participant left meeting
    _meeting.on(
      Events.participantLeft,
      (participantId) {
        final newParticipants = participants;

        newParticipants.remove(participantId);
        setState(() {
          participants = newParticipants;
        });
      },
    );
  }
}
```

6. Creating the `ParticipantTile` which will be placed inside the GridView.

```js title="participant_tile.dart"
import 'package:flutter/material.dart';
import 'package:videosdk/rtc.dart';

class ParticipantTile extends StatefulWidget {
  final Participant participant;
  final bool isLocalParticipant;
  const ParticipantTile(
      {Key? key, required this.participant, this.isLocalParticipant = false})
      : super(key: key);

  @override
  State<ParticipantTile> createState() => _ParticipantTileState();
}

class _ParticipantTileState extends State<ParticipantTile> {
  Stream? videoStream;
  Stream? audioStream;

  @override
  Widget build(BuildContext context) {
    return Container();
  }

}
```

- Now we will initialize the state with the current streams of the Participant and add listeners for stream change.

```js title="participant_tile.dart"
class _ParticipantTileState extends State<ParticipantTile> {
  Stream? videoStream;
  Stream? audioStream;

  @override
  void initState() {
    _initStreamListeners();
    super.initState();

    widget.participant.streams.forEach((key, Stream stream) {
      setState(() {
        if (stream.kind == 'video') {
          videoStream = stream;
        } else if (stream.kind == 'audio') {
          audioStream = stream;
        }
      });
    });
  }

  //... build

  _initStreamListeners() {
    widget.participant.on(Events.streamEnabled, (Stream _stream) {
      setState(() {
        if (_stream.kind == 'video') {
          videoStream = _stream;
        } else if (_stream.kind == 'audio') {
          audioStream = _stream;
        }
      });
    });

    widget.participant.on(Events.streamDisabled, (Stream _stream) {
      setState(() {
        if (_stream.kind == 'video' && videoStream?.id == _stream.id) {
          videoStream = null;
        } else if (_stream.kind == 'audio' && audioStream?.id == _stream.id) {
          audioStream = null;
        }
      });
    });

    widget.participant.on(Events.streamPaused, (Stream _stream) {
      setState(() {
        if (_stream.kind == 'video' && videoStream?.id == _stream.id) {
          videoStream = _stream;
        } else if (_stream.kind == 'audio' && audioStream?.id == _stream.id) {
          audioStream = _stream;
        }
      });
    });

    widget.participant.on(Events.streamResumed, (Stream _stream) {
      setState(() {
        if (_stream.kind == 'video' && videoStream?.id == _stream.id) {
          videoStream = _stream;
        } else if (_stream.kind == 'audio' && audioStream?.id == _stream.id) {
          audioStream = _stream;
        }
      });
    });
  }

}
```

- Now we will create `RTCVideoView` to show the participant stream and also add other components like the name and mic status indicator of the participant.

```js title="participant_tile.dart"

class _ParticipantTileState extends State<ParticipantTile> {
  Stream? videoStream;
  Stream? audioStream;

  @override
  Widget build(BuildContext context) {
    return Container(
      margin: const EdgeInsets.all(4.0),
      decoration: BoxDecoration(
        color: Theme.of(context).backgroundColor.withOpacity(1),
        border: Border.all(
          color: Colors.white38,
        ),
      ),
      child: AspectRatio(
        aspectRatio: 1,
        child: Padding(
          padding: const EdgeInsets.all(4.0),
          child: Stack(
            children: [
              //To Show the participant Stream
              videoStream != null
                  ? RTCVideoView(
                      videoStream?.renderer as RTCVideoRenderer,
                      objectFit:
                          RTCVideoViewObjectFit.RTCVideoViewObjectFitCover,
                    )
                  : const Center(
                      child: Icon(
                        Icons.person,
                        size: 180.0,
                        color: Color.fromARGB(140, 255, 255, 255),
                      ),
                    ),

              //Display the Participant Name
              Positioned(
                bottom: 0,
                left: 0,
                child: FittedBox(
                  fit: BoxFit.scaleDown,
                  child: Container(
                    padding: const EdgeInsets.all(2.0),
                    decoration: BoxDecoration(
                      color: Theme.of(context).backgroundColor.withOpacity(0.2),
                      border: Border.all(
                        color: Colors.white24,
                      ),
                      borderRadius: BorderRadius.circular(4.0),
                    ),
                    child: Text(
                      widget.participant.displayName,
                      textAlign: TextAlign.center,
                      style: const TextStyle(
                        color: Colors.white,
                        fontSize: 10.0,
                      ),
                    ),
                  ),
                ),
              ),

              //Display the Participant Mic Status
              Positioned(
                  top: 0,
                  left: 0,
                  child: InkWell(
                    child: Container(
                      padding: const EdgeInsets.all(4),
                      decoration: BoxDecoration(
                        shape: BoxShape.circle,
                        color: audioStream != null
                            ? Theme.of(context).backgroundColor
                            : Colors.red,
                      ),
                      child: Icon(
                        audioStream != null ? Icons.mic : Icons.mic_off,
                        size: 16,
                      ),
                    )
                  ),
                ),
            ],
          ),
        ),
      ),
    );
  }

  //... _initListener

}
```

:::note

Stuck anywhere? Check out this [example code](https://github.com/videosdk-live/videosdk-rtc-flutter-sdk-example) on GitHub

:::

## Run and Test

The app is all set to test. Make sure to update the `_token` in `join_screen.dart`

Your app should look like this after the implementation.

![VideoSDK Flutter Quick Start Join Screen](/img/quick-start/flutter-join-screen.jpg) ![VideoSDK Flutter Quick Start Meeting Screen](/img/quick-start/flutter-meeting-screen.jpg)

:::caution
For the tutorial purpose, we used a static token to initialize and join the meeting. But for the production version of the app, we recommend you use an Authentication Server that will generate and pass on the token to the Client App. For more details checkout [how to do server setup](/flutter/guide/video-and-audio-calling-api-sdk/server-setup).
:::
