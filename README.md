# firebaseauth
Great — if you want to **build this Flutter app with Firebase Auth and a 5-tab bottom navigation bar using the CLI**, here's a complete step-by-step guide **using only the terminal/CLI** (plus code snippets).

---

## ✅ Step-by-Step: Flutter + Firebase Auth + 5 Screens (CLI Only)

---

### 🔧 1. **Create Flutter App**

```bash
flutter create my_app
cd my_app
```

---

### 🔥 2. **Install Firebase CLI & Configure Project**

#### Install Firebase CLI:

```bash
npm install -g firebase-tools
```

#### Login:

```bash
firebase login
```

#### Initialize Firebase in your project:

```bash
firebase init
```

Choose:

* **Authentication**
* **Flutter (Web optional)**

You can skip Firestore, Hosting, etc. for now.

---

### 📦 3. **Install Flutter Firebase Packages**

Update `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.30.0
  firebase_auth: ^4.17.7
```

Then run:

```bash
flutter pub get
```

---

### 📁 4. **Set Up Firebase Android App**

In CLI:

```bash
firebase apps:create android com.example.my_app
firebase apps:sdkconfig android
```

You'll get a `google-services.json` — move it to:

```
android/app/google-services.json
```

Then:

#### Edit `android/build.gradle`:

```gradle
classpath 'com.google.gms:google-services:4.3.15'
```

#### Edit `android/app/build.gradle`:

```gradle
apply plugin: 'com.google.gms.google-services'
```

---

### 🚀 5. **Initialize Firebase in Code**

In `main.dart`:

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'main_screen.dart';
import 'login_screen.dart';
import 'auth_service.dart';
import 'package:firebase_auth/firebase_auth.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: StreamBuilder<User?>(
        stream: AuthService.userStream,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting)
            return const Center(child: CircularProgressIndicator());
          return snapshot.hasData ? const MainScreen() : const LoginScreen();
        },
      ),
    );
  }
}
```

---

### 🧠 6. **Create Auth Service**

**Create `lib/auth_service.dart`:**

```dart
import 'package:firebase_auth/firebase_auth.dart';

class AuthService {
  static final FirebaseAuth _auth = FirebaseAuth.instance;

  static Stream<User?> get userStream => _auth.authStateChanges();

  static Future<void> signOut() => _auth.signOut();

  static Future<void> signIn(String email, String password) async {
    await _auth.signInWithEmailAndPassword(email: email, password: password);
  }

  static Future<void> register(String email, String password) async {
    await _auth.createUserWithEmailAndPassword(email: email, password: password);
  }
}
```

---

### 🧾 7. **Create Screens Folder**

```bash
mkdir lib/screens
touch lib/screens/home.dart lib/screens/explore.dart lib/screens/notifications.dart lib/screens/profile.dart lib/screens/settings.dart
```

Paste this into each screen file:

```dart
import 'package:flutter/material.dart';

class ScreenName extends StatelessWidget {
  const ScreenName({super.key});

  @override
  Widget build(BuildContext context) {
    return const Scaffold(
      body: Center(child: Text("ScreenName")),
    );
  }
}
```

Replace `ScreenName` with each actual screen name (`HomeScreen`, `ExploreScreen`, etc).

---

### 🧭 8. **Create Bottom Navigation UI**

**Create `lib/main_screen.dart`:**

```dart
import 'package:flutter/material.dart';
import 'screens/home.dart';
import 'screens/explore.dart';
import 'screens/notifications.dart';
import 'screens/profile.dart';
import 'screens/settings.dart';
import 'auth_service.dart';

class MainScreen extends StatefulWidget {
  const MainScreen({super.key});
  @override
  State<MainScreen> createState() => _MainScreenState();
}

class _MainScreenState extends State<MainScreen> {
  int _currentIndex = 0;
  final List<Widget> _screens = const [
    HomeScreen(),
    ExploreScreen(),
    NotificationsScreen(),
    ProfileScreen(),
    SettingsScreen(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("My App"),
        actions: [
          IconButton(
            icon: const Icon(Icons.logout),
            onPressed: () => AuthService.signOut(),
          ),
        ],
      ),
      body: _screens[_currentIndex],
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _currentIndex,
        onTap: (index) => setState(() => _currentIndex = index),
        type: BottomNavigationBarType.fixed,
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.home), label: "Home"),
          BottomNavigationBarItem(icon: Icon(Icons.explore), label: "Explore"),
          BottomNavigationBarItem(icon: Icon(Icons.notifications), label: "Alerts"),
          BottomNavigationBarItem(icon: Icon(Icons.person), label: "Profile"),
          BottomNavigationBarItem(icon: Icon(Icons.settings), label: "Settings"),
        ],
      ),
    );
  }
}
```

---

### 🔐 9. **Add Auth Screens**

```bash
touch lib/login_screen.dart lib/register_screen.dart
```

Fill in simple UI with `TextFields` and buttons for login/registration using `AuthService.signIn()` and `register()`.

---

### ✅ 10. **Run Your App**

```bash
flutter run
```

---

## ✅ Summary: CLI Workflow

| Task           | Command                                         |
| -------------- | ----------------------------------------------- |
| Create app     | `flutter create my_app`                         |
| Add Firebase   | `firebase init`, `firebase apps:create android` |
| Add packages   | `flutter pub get`                               |
| Create screens | `touch` or use `mkdir`                          |
| Run app        | `flutter run`                                   |

---

If you want, I can generate the actual full file content for `login_screen.dart` and `register_screen.dart`, or zip a full example app you can run instantly. Want that?
