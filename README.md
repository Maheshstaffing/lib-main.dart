import 'package:flutter/material.dart';
import 'package:file_picker/file_picker.dart';
import 'package:url_launcher/url_launcher.dart';

void main() => runApp(MaheshStaffingApp());

class MaheshStaffingApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Mahesh Staffing Services',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  final String whatsappUrl = "https://chat.whatsapp.com/Ca1pnx1KHYaAKCd9Brqg6a";

  void _launchURL(String url) async {
    if (!await launchUrl(Uri.parse(url))) {
      throw 'Could not launch $url';
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Mahesh Staffing Services')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.business_center, size: 100, color: Colors.blue),
            SizedBox(height: 20),
            Text('Welcome to Mahesh Staffing Services',
                style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () => Navigator.push(
                  context, MaterialPageRoute(builder: (_) => JobsPage())),
              child: Text('View Job Openings'),
            ),
            ElevatedButton(
              onPressed: () => Navigator.push(
                  context, MaterialPageRoute(builder: (_) => ApplyPage())),
              child: Text('Apply Now'),
            ),
            ElevatedButton(
              onPressed: () => Navigator.push(
                  context, MaterialPageRoute(builder: (_) => ContactPage())),
              child: Text('Contact Us'),
            ),
            SizedBox(height: 20),
            ElevatedButton.icon(
              icon: Icon(Icons.chat),
              label: Text('Chat on WhatsApp'),
              onPressed: () => _launchURL(whatsappUrl),
            )
          ],
        ),
      ),
    );
  }
}

class JobsPage extends StatelessWidget {
  final List<String> jobs = [
    'Front Desk Receptionist',
    'Data Entry Operator',
    'HR Executive',
    'Software Developer'
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Job Openings')),
      body: ListView.builder(
        itemCount: jobs.length,
        itemBuilder: (context, index) {
          return ListTile(
            leading: Icon(Icons.work),
            title: Text(jobs[index]),
          );
        },
      ),
    );
  }
}

class ApplyPage extends StatefulWidget {
  @override
  _ApplyPageState createState() => _ApplyPageState();
}

class _ApplyPageState extends State<ApplyPage> {
  final TextEditingController nameController = TextEditingController();
  final TextEditingController phoneController = TextEditingController();
  String? fileName;

  Future<void> pickFile() async {
    FilePickerResult? result = await FilePicker.platform.pickFiles();
    if (result != null) {
      setState(() {
        fileName = result.files.single.name;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Apply Now')),
      body: Padding(
        padding: EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: nameController,
              decoration: InputDecoration(labelText: 'Full Name'),
            ),
            TextField(
              controller: phoneController,
              decoration: InputDecoration(labelText: 'Phone Number'),
              keyboardType: TextInputType.phone,
            ),
            SizedBox(height: 20),
            ElevatedButton(
              child: Text('Upload Resume'),
              onPressed: pickFile,
            ),
            if (fileName != null) Text('Selected File: $fileName'),
            SizedBox(height: 20),
            ElevatedButton(
              child: Text('Submit Application'),
              onPressed: () {
                ScaffoldMessenger.of(context).showSnackBar(
                  SnackBar(content: Text('Application submitted successfully!')),
                );
              },
            ),
          ],
        ),
      ),
    );
  }
}

class ContactPage extends StatelessWidget {
  final String phone = "tel:+919154477414";
  final String email = "mailto:maheshstaffingservices@gmail.com";

  void _launchURL(String url) async {
    if (!await launchUrl(Uri.parse(url))) {
      throw 'Could not launch $url';
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Contact Us')),
      body: Padding(
        padding: EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            ListTile(
              leading: Icon(Icons.phone),
              title: Text('Phone: +91 91544 77414'),
              onTap: () => _launchURL(phone),
            ),
            ListTile(
              leading: Icon(Icons.email),
              title: Text('Email: maheshstaffingservices@gmail.com'),
              onTap: () => _launchURL(email),
            ),
            ListTile(
              leading: Icon(Icons.location_on),
              title: Text('Location: Uppal, Hyderabad'),
            ),
          ],
        ),
      ),
    );
  }
}
