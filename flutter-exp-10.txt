import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

void main() {
  runApp(const ProfileApp());
}

class UserProfileData extends ChangeNotifier {
  String _name = '';

  String get name => _name;

  void updateName(String newName) {
    _name = newName;
    notifyListeners();
  }
}

class ProfileApp extends StatelessWidget {
  const ProfileApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider<UserProfileData>(
      create: (context) => UserProfileData(),
      builder: (context, child) => MaterialApp(
        home: const ProfileScreen(),
      ),
    );
  }
}

class ProfileScreen extends StatelessWidget {
  const ProfileScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final userData = Provider.of<UserProfileData>(context);

    return Scaffold(
      appBar: AppBar(title: const Text('User Management')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Text(
              userData.name.isEmpty ? 'Welcome!' : 'Welcome, ${userData.name}!',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
            const SizedBox(height: 20),
            TextField(
              key: const Key('nameInput'),
              decoration: const InputDecoration(labelText: 'Enter Name'),
              onChanged: (value) => userData.updateName(value),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              key: const Key('openProfileButton'),
              onPressed: () {
                Navigator.of(context).push(
                  MaterialPageRoute(
                    builder: (context) => ProfileDetailsPage(name: userData.name),
                  ),
                );
              },
              child: const Text('Open Profile'),
            ),
          ],
        ),
      ),
    );
  }
}

class ProfileDetailsPage extends StatelessWidget {
  final String name;

  const ProfileDetailsPage({super.key, required this.name});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Profile Details')),
      body: Center(
        child: Text(
          'Profile: $name',
          style: Theme.of(context).textTheme.headlineSmall,
        ),
      ),
    );
  }
}