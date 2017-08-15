---
layout: page
title: Flutter for Android Developers
permalink: /flutter-for-android/
---
This document is meant for Android developers looking to transfer their existing Android knowledge to build mobile apps in Flutter. If you understand the fundamentals of the Android framework then you can use this document as a jump start to Flutter development.

You can use this document as a cookbook by jumping around and finding questions that are most relevant to your need. 

# Views

**What is the equivalent of a View in Flutter?**

In Android, the View is the foundation of everything that shows up on the screen. From Buttons, Toolbars, and Inputs, everything is a View.  In Flutter the equivalent of a View would be Widget. However, Widgets have a few differences when compared with a View. To start, widgets only last for a frame, and on every frame, we create a tree of widget instances. In comparison, on Android when a View is drawn it does not redraw until invalidate is called. 

This is because unlike Android’s view hierarchy system where we have Views and we mutate those views on the view hierarchy, Widgets in Flutter are immutable, this allows Widgets to be super lightweight.



**How do I update Widgets?** 

This is where the concept of Stateful vs Stateless widgets comes from. A StatelessWidget is just what it sounds like, a widget with no state information. StatelessWidgets are useful when the part of the user interface you are describing does not depend on anything other than the configuration information in the object.

For example, in Android, this would be similar to placing a TextView with static text, or a logo, that isn't changing based on user interaction or network data.

If you want to dynamically change the UI based on network data or user interaction then you have to work with StatefulWidget and tell the Flutter framework that the widget’s State has been updated so it can update that widget.

The important thing to note here is at the core both Stateless and Stateful widgets behave the same. They rebuild every frame, the difference is the StatefulWidget has a State object which stores state data across frames and restores it.

If you are in doubt, then always remember this rule: If a widget changes—the user interacts with it, for example—it’s stateful.

Let's take a look at how you would use a StatelessWidget. A common StatelessWidget is a Text widget. If you look at the implemtnation of the Text Widget you'll find it subclasses a StatelessWidget

<!-- skip -->
{% prettify dart %}
new Text(
  'I like Flutter!',
  style: new TextStyle(fontWeight: FontWeight.bold),
);
{% endprettify %}

As you can see, the Text Widget has no state information associated with it, it renders what is passed in it's constructors and nothing more. 

However, what if you want to make "I Like Flutter" change dynamically, for example from clicking a FloatingActionButton?

This can be acheived by wrapping the Text widget in a StatefulWidget and updating it when the button is clicked.

For example:

<!-- skip -->
{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(new SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Sample App',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => new _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  // Default placeholder text
  String textToShow = "I Like Flutter";
  void _updateText() {
    setState(() {
      // update the text
      textToShow = "Flutter is Awesome!";
    });
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("Sample App"),
      ),
      body: new Center(child: new Text(textToShow)),
      floatingActionButton: new FloatingActionButton(
        onPressed: _updateText,
        tooltip: 'Update Text',
        child: new Icon(Icons.update),
      ),
    );
  }
}
{% endprettify %}



**How do I layout my Widgets? Where is my XML layout file?**

In Android, you write layouts via XML, however in Flutter you write your layouts with a widget tree. 

Here is an example of how you would display a simple Widget on the screen and add some padding to it.

<!-- skip -->
{% prettify dart %}
@override
Widget build(BuildContext context) {
  return new Scaffold(
    appBar: new AppBar(
      title: new Text("Sample App"),
    ),
    body: new Center(
      child: new MaterialButton(
        onPressed: () {},
        child: new Text('Hello'),
        padding: new EdgeInsets.only(left: 10.0, right: 10.0),
      ),
    ),
  );
}
{% endprettify %}



**How do I add or remove a component from my layout?**

In Android, you would call addChild or removeChild from a parent to dynamically add or remove views from a parent. In Flutter, because widgets are immutable there is no addChild, instead, you can simply pass in a function that returns a widget to the parent and control that child's creation via a boolean.

For example here is how you can toggle between two widgets when you click on a FloatingActionButton

<!-- skip -->
{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(new SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Sample App',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => new _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  // Default value for toggle
  bool toggle = true;
  void _toggle() {
    setState(() {
      toggle = !toggle;
    });
  }

  _getToggleChild() {
    if (toggle) {
      return new Text('Toggle One');
    } else {
      return new MaterialButton(onPressed: () {}, child: new Text('Toggle Two'));
    }
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("Sample App"),
      ),
      body: new Center(
        child: _getToggleChild(),
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: _toggle,
        tooltip: 'Update Text',
        child: new Icon(Icons.update),
      ),
    );
  }
}
{% endprettify %}



**In Android, I can Animate a view by View.animate(), how can I do that to a Widget?**
In Flutter, animating widgets can be done easily via the animation library. See https://flutter.io/widgets/animation/

Let's take a look at how to write a FadeTransition

[code]



**How do I build custom Widgets?**

Building custom widgets in Flutter is as simple as extending a Widget and adding your custom logic. This is like Android where you would extend a View and override the methods to customize it.

Let's take a look at how to build a custom button

[code]



# Intents

**What is the equivalent of an Intent in Flutter?**

Flutter does not have the concept of Intents. However in Android, there are two main use-cases for Intents, One is to switch between Activities and another is to invoke external(or internal) components of your app (Camera, Services etc).

To switch between screens in Flutter you can directly access the router to draw a new Widget.

[code]

Now in Android, you can pass some data along with the Intent, while the Navigation router does not have a way to pass data directly, you can approach this problem by creating a static “data” class that you can store and retrieve values when switching screens. 

There is also a very popular Router called “Fluro” that adds this functionality for you.

[code]

The other popular use-case for Intents is to call external components such as a Camera or File picker. For this, you would need to create a native platform integration (or use an existing library)

See [Flutter Plugins] to learn how to build a native platform integration.



**How do I handle incoming Intents from external applications in Flutter?** 

This can be achieved by registering the intent you want to handle in the AndroidMainfest.xml to launch MainActivity (the default activity created by Flutter)

[code]

Then in MainActivity you can handle the intent and pass it to Flutter 

[code]

Lastly, in Flutter you can receive this via

[code]

# Async UI

**What is the equivalent of runOnUiThread in Flutter?**

Because of Flutters reactive architecture, there is no need to worry about if you are running stuff on the UI thread, as long as you make sure you don't block your network calls.

For example, when making long running calls you should always follow the async/await pattern

[code]

To update the UI you would call setState which would trigger the build method to run again and update the data.

[code]



**What is the equivalent of an AsyncTask or IntentService on Android?**

In many cases, you do not have to worry about threading in Flutter as long as you follow the async/await patterns. This is because Flutter has a single-threaded event loop (similar to Node.js).

To run code asynchronously you can declare the function as an async function and await on long running tasks in the function

[code]

This is where you would typically do network or database calls. 

With regards to AsyncTask which follows a three step model onPreExecute, doInBackground and onPostExecute, doing a similar model in Flutter is much simpler. You can just write non-blocking code as it is like you would in onPreExecute and onPostExecute and to run code asynchronously you would call an async function

[code]

onPostExecute typically takes the value from the result of the doInBackground task and updates the UI, in Flutter as you can see above you would just take the value returned from the await’ed function and update the UI by calling setState.

However, there are times where you may be processing a large amount of data and your UI will hang. 

To get around this you can run code in the background. See <u>*How do I run parallel code on multi-core devices? I.e AsyncTask.execute(AsyncTask.THREAD_POOL_EXECUTOR) or run code in a background thread?*</u>



**How do I run parallel code on multi-core devices? I.e AsyncTask.execute(AsyncTask.THREAD_POOL_EXECUTOR) or run code in a background thread?**

Since Flutter uses a single threaded execution model, the code you write normally executes on a single core on the mobile device. 

However, it is possible to take advantage of multiple CPU cores to do long running or computationally intensive tasks. This is done by using Isolates.

Isolates are a separate execution thread that runs and do not share any memory with the main execution thread. This means you can’t just access variables from the main thread or update your UI by calling setState. 

Let's see an example of a simple Isolate and how you can communicate and share data back to the main thread to update your UI.

[code]

While it’s no golden rule, try to keep your Isolates to the number of CPU cores on the device as having too many will cause your execution to slow down. It is also preferred to use coarse-grained communications with Isolates.

For example:

[code]

**What is the equivalent of OkHttp on Flutter?**

Making a network call in Flutter is very easy when you use the popular "http" package from https://pub.dartlang.org/packages/http

You can use it by adding it to your dependencies in pubspec.yaml

[code]

Then to make a network call, for example to this JSON GIST on GitHub you can call

[code]

Once you have the result you can tell Flutter to update its state by calling setState like so

[code]

Which will update your UI with the result from your network call.

**How do I show progress indicator on android when there is a task that is running?**

Before you call your long running task, you can show a Indicator to let the user know there is processing happening. This can be done by rendering a Dialog widget. You can show the Dialog programmatically by controlling it's rendering through a boolean and telling Flutter to update it's state just before your long running task.

[code]

# Project Structure & Resources

**Where do I store my resolution dependent image files? HDPI/XXHDPI**

Flutter follows a simple 3 resolution format like iOS. 1x, 2x, and 3x.

Create a folder called images and for each of your image files, generate a @2x and @3x variant and place them in the folder like such

- …/my_icon.png

- …/2.0x/my_icon.png
- …/3.0x/my_icon.png

Then you would need to declare these images in your pubspec.yaml file

[code]

You can then access your images using AssetImage

[code]

**Where do I store strings? How do I store different locales**

Currently, best practice is to create a class called Strings that is locale aware, for example

[code]

Then in your code, you can access your Strings as such

[code]

**What is the equivalent of a Gradle file to add my external dependencies?**

While there are Gradle files under the Android folder, you would only use these if you are adding dependencies needed for platform integration. Otherwise, you can use pubspec.yaml to declare external dependencies.

A good place to find great packages for flutter is pub.dartlang.org

[code]

# Activities and Fragments

**What are the equivalent of activities and fragments?**

In Android, an Activity represents a single focused thing the user can do. A Fragment represents a better way to modularize your code, build more sophisticated user interfaces for larger screens, and help scale your application between small and large screens. In Flutter both of these concepts fall under the concept of a Widget.

Since Widgets are nested in Flutter, you can consider the top level parent widget to be an "Activity", and if you architect your child widgets correctly so that they can scale and be re-used, you can consider those "Fragments".

**How do I listen to Android Activity lifecycle events?**

You can easily listen to lifecycle events by hooking into the WidgetsBinding observer and listening to the didChangeAppLifecycleState change event.

[code]

# Layouts

**What is the equivalent of a LinearLayout?**

A LinearLayout is commonly used to lay your widgets out linearly horizontally or vertically. In Flutter, you can use the Row widget or Column widget to achieve the same result.

[Example of Row]

[Example of Column]

**What is the equivalent of a RelativeLayout?**

A RelativeLayout is used to lay your widgets out relative to each other. In Flutter there are a few ways to achieve the same result.

You can achieve the result of a RelativeLayout by using a combination of Column, Row, and Stack widgets. You can specify rules for the widgets constructors on how the children are laid out relative to the parent.

[Simple example]

You can also use the more complex but powerful CustomMultiChildLayout to achieve a RelativeLayout.

[complex code]

**What is the equivalent of a ScrollView?**

A ScrollView lets you lay your widgets such that if the users' device has a smaller screen than your content, they can scroll.

In Flutter the easiest way to do this is using the ListView widget. This might seem like overkill coming from Android, but in Flutter a ListView widget is both a ScrollView and an Android ListView.

[code]

# Gesture Detection and Touch Event Handling

**How do I add an onClick listener to a widget in Flutter?**

Typically you would want to add event listeners to your widgets, such as a button or image. In Flutter there are two easy ways of doing this.

1. If the Widget has support for event detection you can just pass in a function to it and handle it. For example, the MaterialButton has an onPressed parameter

   ​  [code]

2. If the Widget does not have support for event detection, you can wrap up the widget in a GestureDetector and pass in a function to the onTap parameter.

   ​  [code]

**How do I handle other gestures on widgets?**

Using the GuestureDetector we can listen to a wide range of Gestures such as

- Tap

  - `onTapDown` A pointer that might cause a tap has contacted the screen at a particular location.
  - `onTapUp` A pointer that will trigger a tap has stopped contacting the screen at a particular location.
  - `onTap` A tap has occurred.
  - `onTapCancel` The pointer that previously triggered the `onTapDown` will not end up causing a tap.

- Double tap

  - `onDoubleTap` The user has tapped the screen at the same location twice in quick succession.

- Long press

  - `onLongPress` A pointer has remained in contact with the screen at the same location for a long period of time.

- Vertical drag

  - `onVerticalDragStart` A pointer has contacted the screen and might begin to move vertically.
  - `onVerticalDragUpdate` A pointer that is in contact with the screen and moving vertically has moved in the vertical direction.
  - `onVerticalDragEnd` A pointer that was previously in contact with the screen and moving vertically is no longer in contact with the screen and was moving at a specific velocity when it stopped contacting the screen.

- Horizontal drag

  - `onHorizontalDragStart` A pointer has contacted the screen and might begin to move horizontally.
  - `onHorizontalDragUpdate` A pointer that is in contact with the screen and moving horizontally has moved in the horizontal direction.
  - `onHorizontalDragEnd` A pointer that was previously in contact with the screen and moving horizontally is no longer in contact with the screen and was moving at a specific velocity when it stopped contacting the screen.

  [code]


# Listviews & Adapters

**What is the alternative to a ListView in Flutter?**

The equivalent to a ListView in Flutter is … a ListView! 

Though the differences are great, the end result is the same. In an Android ListView, you create an adapter that you can then pass into the ListView which will render each row with what your adapter returns. However you have to make sure you recycle your rows properly, otherwise, you get all sorts of crazy visual glitches and memory issues.

In Flutter, due to Flutters immutable widget pattern, you simply pass in a List of Widgets to your ListView and Flutter will take care of making sure they are scrolling fast and smooth.

<!-- skip -->
{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(new SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Sample App',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => new _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("Sample App"),
      ),
      body: new ListView(children: _getListData()),
    );
  }

  _getListData() {
    List<Widget> widgets = new List();
    for (int i = 0; i < 100; i++) {
      widgets.add(new Padding(padding: new EdgeInsets.all(10.0), child: new Text("Row $i")));
    }
    return widgets;
  }
}
{% endprettify %}

**How do I know which list item is clicked on?**

Unlike Android where a ListView offers an onItemClickListener, Flutter makes it a lot easier by letting you just use the touch handling that the widgets you passed in have.

<!-- skip -->
{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(new SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Sample App',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => new _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("Sample App"),
      ),
      body: new ListView(children: _getListData()),
    );
  }

  _getListData() {
    List<Widget> widgets = new List();
    for (int i = 0; i < 100; i++) {
      widgets.add(new GestureDetector(
        child: new Padding(padding: new EdgeInsets.all(10.0), child: new Text("Row $i")),
        onTap: () {
          print('row tapped');
        },
      ));
    }
    return widgets;
  }
}
{% endprettify %}

**How do I update ListView's dynamically?**

This confuses a lot of people when coming from Android. On Android, you would update the adapter and call notifyDataSetChanged. In Flutter if you were to update the list of widgets inside a setState(), you would quickly see that your data did not change visually. 

This is because when setState is called, the Flutter rendering engine will go through all the widgets to see if they have changed. When it gets to your ListView it will do a `==operator` and see that the two ListViews are the same and nothing has changed, hence no update to the data.

The quick and dirty way around this is to assign your list variable a new List() inside of setState, copy over all the old data to the new list. This is inefficient a simple dirty way to acheive an update. 

<!-- skip -->
{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(new SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Sample App',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => new _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = new List();

  @override
  void initState() {
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("Sample App"),
      ),
      body: new ListView(children: widgets),
    );
  }

  Widget getRow(int i) {
    return new GestureDetector(
      child: new Padding(padding: new EdgeInsets.all(10.0), child: new Text("Row $i")),
      onTap: () {
        setState(() {
          widgets = new List.from(widgets);
          widgets.add(getRow(widgets.length + 1));
          print('row $i');
        });
      },
    );
  }
}
{% endprettify %}

However the recommended, more efficent and effective way is to use a ListView.Builder

<!-- skip -->
{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(new SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Sample App',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => new _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = new List();

  @override
  void initState() {
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
        appBar: new AppBar(
          title: new Text("Sample App"),
        ),
        body: new ListView.builder(
            itemCount: widgets.length,
            itemBuilder: (BuildContext context, int position) {
              return getRow(position);
            }));
  }

  Widget getRow(int i) {
    return new GestureDetector(
      child: new Padding(padding: new EdgeInsets.all(10.0), child: new Text("Row $i")),
      onTap: () {
        setState(() {
          widgets.add(getRow(widgets.length + 1));
          print('row $i');
        });
      },
    );
  }
}
{% endprettify %}

Instead of creating a "new ListView" we create a new ListView.builder which takes two key paramters, the initial length of the list and an Itembuilder function.

The ItemBuilder function is a lot like the getView function in an Android adapter, it takes in a position and you return the row you want rendered for that position.

Lastly, but most important, if you notice the onTap function, we don't recreate the List anymore and instead just .add to it.



# Working with Text

**How do I set custom fonts on my Text widgets?**

Customizing the Font of a Text widget is simple in Flutter, first you need to take your font file and place in folder in your project (best practice is to create a folder called assets)

Next in your pubspec.yaml file you would declare the fonts

{% prettify yaml %}
fonts:
   - family: MyCustomFont
     fonts:
       - asset: fonts/MyCustomFont.ttf
       - style: italic
{% endprettify %}

and lasty you would assign the font to your Text widget

<!-- skip -->
{% prettify dart %}
@override
Widget build(BuildContext context) {
  return new Scaffold(
    appBar: new AppBar(
      title: new Text("Sample App"),
    ),
    body: new Center(
      child: new Text(
        'This is a custom font text',
        style: new TextStyle(fontFamily: 'MyCustomFont'),
      ),
    ),
  );
}
{% endprettify %}

**How do I style my Text widgets?**

Along with customizing fonts you can customize a lot of different styles on a Text widget.

The style parameter of a Text widget takes a TextStyle object, where you can customize many paramters such as

color
decoration
decorationColor
decorationStyle
fontFamily
fontSize
fontStyle
fontWeight
hashCode
height
inherit
letterSpacing
textBaseline
wordSpacing

# Form Input

**What is the equivalent of a "hint" on an Input?**

In Flutter you can easily show a "hint" or a placeholder text for your input by adding an InputDecoration object to the decoration constructor parameter for the Text Widget

<!-- skip -->
{% prettify dart %}
body: new Center(
  child: new TextField(
    decoration: new InputDecoration(hintText: "This is a hint"),
  )
)
{% endprettify %}

**How do I show validation errors?**

Just like how you would with a "hint", you can pass in a InputDecoration object to the decoration constructor for the Text widget.

However, you would not want to start off with showing an error and typically would want to show it when the user has entered some invalid data. This can be done by updating the state and passing in a new InputDecoration object.

<!-- skip -->
{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(new SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Sample App',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => new _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  String _errorText;

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("Sample App"),
      ),
      body: new Center(
        child: new TextField(
          onSubmitted: (String text) {
            setState(() {
              if (!isEmail(text)) {
                _errorText = 'Error: This is not an email';
              } else {
                _errorText = null;
              }
            });
          },
          decoration: new InputDecoration(hintText: "This is a hint", errorText: _getErrorText()),
        ),
      ),
    );
  }

  _getErrorText() {
    return _errorText;
  }

  bool isEmail(String em) {
    String p =
        r'^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$';

    RegExp regExp = new RegExp(p);

    return regExp.hasMatch(em);
  }
}
{% endprettify %}



# Flutter Plugins

**How do I access the GPS sensor?**

**How do I access the Camera?**

**How do I log in with Facebook**

**How do I build my own custom native integrations?**

# Themes

**How do I theme my app?**

Flutter out of the box comes with a precise implementation of Material Design, which takes care of a lot of styling and theming needs that you would typically do.

To take full advantage of MaterialDesign in your app, you can declare a top level widget `MaterialApp` as the entry point to your application. This is where you would declare routing and navigation as well

[code]

To customize the colors and styles of Material Design you can pass in a ThemeData object to the MaterialApp widget



# Common Widget Map


