# UTS2019102042
Pengembangan Aplikasi Mobile Lanjut
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:async';
import 'dart:convert';

void main() => runApp(MyApp());
String url = "http://localhost/phpmyadmin/sql.php?db=dbkaryawan&table=employees&token=179b89685fff57f7f041e2a84d084859&pos=0"

 // For Http Request Here ok bro
 Future<Post> fecthPost() async {
   final response = await http.get(url);
   if (response.statuscode == 200) {
     return post.fromJson(json.decode(response.body));
   }else{
     throw Exception('Failed to make request');
   }
   }
   // Make Claa Post Model
   class Post {
    final int userId;
    final int id;
    final String title;
    final String body;

Post({this.userId, this.id, this.title, this.body});
factory Post.fromJson(Map<String, dynamic> json) {
return Post(
userId: json['userID'],
 id: json['id'],
 title: json['title'],
 body: json['body']
 );

 }

 }

 class MyApp extends StatelessWidget {
// This widget is the root of your application.
 @override
 Widget build(BuildContext context) {
 return MaterialApp(
 title: 'Flutter Demo',
 theme: ThemeData(
 // This is the theme of your application.
 //
 // Try running your application with "flutter run". You'll see the
 // application has a blue toolbar. Then, without quitting the app, try
// changing the primarySwatch below to Colors.green and then invoke
// "hot reload" (press "r" in the console where you ran "flutter run",
// or simply save your changes to "hot reload" in a Flutter IDE).
// Notice that the counter didn't reset back to zero; the application
// is not restarted.
primarySwatch: Colors.red,
),
 home: MyHomePage(title: 'Get API'),
);
 }
 }

 class MyHomePage extends StatefulWidget {
 MyHomePage({Key key, this.title}) : super(key: key);

// This widget is the home page of your application. It is stateful, meaning
// that it has a State object (defined below) that contains fields that affect
// how it looks.

// This class is the configuration for the state. It holds the values (in this
// case the title) provided by the parent (in this case the App widget) and
// used by the build method of the State. Fields in a Widget subclass are
// always marked "final".

 final String title;

 @override
_MyHomePageState createState() => _MyHomePageState();
 }

 class _MyHomePageState extends State<MyHomePage> with TickerProviderStateMixin {
 // Make constructor
 final Future<Post> post = fecthPost() ;

@override
Widget build(BuildContext context) {
// This method is rerun every time setState is called, for instance as done
// by the _incrementCounter method above.
//
// The Flutter framework has been optimized to make rerunning build methods
// fast, so that you can just rebuild anything that needs updating rather
// than having to individually change instances of widgets.
return Scaffold(
 appBar: AppBar(
// Here we take the value from the MyHomePage object that was created
by
// the App.build method, and use it to set our appbar title.
 title: Text(widget.title),
 ),
 body: Center(
 child: FutureBuilder<Post>(
 future: post,
 builder: (context, snapshot){
 if(snapshot.hasData){
 print(snapshot.data.title);
 return Text(snapshot.data.title, textAlign: TextAlign.center, style: TextStyle(fontWeight: FontWeight.bold, color: Colors.red, fontSize: 2
));
 }
 else if(snapshot.hasError){
 return Text("${snapshot.error}", style: TextStyle(fontWeight: FontWeight.bold),);
}
 else{
 return CircularProgressIndicator(backgroundColor: Colors.amber,);
 }
},

 ),
 )
 );
 }
 }
