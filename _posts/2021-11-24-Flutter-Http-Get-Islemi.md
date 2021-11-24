---
layout: post
title: "Flutter - http ile Get İşlemi"
categories: misc
---
**JSON**

```json
{
  "activity": "Watch the sunset or the sunrise",
  "type": "recreational",
  "participants": 1,
  "price": 0,
  "link": "",
  "key": "4748214",
  "accessibility": 1
}
```

**bored_service.dart**

```dart
import 'dart:convert' as convert;
import 'package:http/http.dart' as http;
import '../models/bored_model.dart';

class Service {
  Future<BoredModel> getData() async {
    var url = Uri.https('boredapi.com', '/api/activity');

    var response = await http.get(url);
    var jsonResponse =
        convert.jsonDecode(response.body) as Map<String, dynamic>;
    return BoredModel.fromJson(jsonResponse);
  }
}
```

**bored_model.dart**

```dart
import 'dart:convert';

BoredModel boredModelFromJson(String str) =>
    BoredModel.fromJson(json.decode(str));

String boredModelToJson(BoredModel data) => json.encode(data.toJson());

class BoredModel {
  BoredModel({
    required this.activity,
    required this.type,
    required this.participants,
    required this.price,
    required this.link,
    required this.key,
    required this.accessibility,
  });

  String activity;
  String type;
  int participants;
  double price;
  String link;
  String key;
  double accessibility;

  factory BoredModel.fromJson(Map<String, dynamic> json) => BoredModel(
        activity: json["activity"],
        type: json["type"],
        participants: json["participants"],
        price: json["price"].toDouble(),
        link: json["link"],
        key: json["key"],
        accessibility: json["accessibility"].toDouble(),
      );

  Map<String, dynamic> toJson() => {
        "activity": activity,
        "type": type,
        "participants": participants,
        "price": price,
        "link": link,
        "key": key,
        "accessibility": accessibility,
      };
}
```

**home.dart**

```dart
import 'package:flutter/material.dart';
import 'package:flutter_api_deneme/widgets/bored_widget.dart';
import '../services/api/bored_service.dart';
import '../services/models/bored_model.dart';

class Home extends StatefulWidget {
  const Home({Key? key}) : super(key: key);

  @override
  _HomeState createState() => _HomeState();
}

class _HomeState extends State<Home> {
  Service apiManager = Service();
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: FutureBuilder(
          future: apiManager.getData(),
          builder: (context, AsyncSnapshot<BoredModel> snapshot) {
            if (snapshot.connectionState == ConnectionState.done) {
              final _data = snapshot.data;
              return BoredWidget(data: _data);
            }
            return const Center(child: CircularProgressIndicator());
          }),
    );
  }
}
```

