# UniqueList - A Dart/Flutter Package for Managing Unique Lists

`UniqueList` is a utility package that ensures uniqueness in your lists based on a custom condition you define. It's perfect for managing collections of objects that must adhere to specific uniqueness rules.

## Features

- 🆕 **Unique List Management**: Add, insert, or remove items while ensuring no duplicates based on a custom condition.
- 🚀 **Fast Operations**: Optimized operations for adding, removing, and filtering items.
- 🔄 **Replace & Modify**: Replace items by condition or unique value.
- 🔍 **Check for Existence**: Quickly check if an item exists based on the uniqueness condition.
- 📦 **Unmodifiable and Modifiable Access**: Retrieve unmodifiable and modifiable versions of the list as needed.

## Getting Started

To start using `UniqueList`, you need to add the following dependency to your `pubspec.yaml` file:

```yaml
dependencies:
  unique_list: ^0.0.1
```

## Usage

```dart
import 'dart:developer';

import 'package:unique_list/unique_list.dart';

class User {
  final int id;
  final String name;

  User(this.id, this.name);

  // Override equality operator and hashCode for proper comparison
  @override
  bool operator ==(Object other) =>
      identical(this, other) ||
          (other is User && id == other.id && name == other.name);

  @override
  int get hashCode => Object.hash(id, name);
}

void main() {
  // Define a unique condition (in this case, users are unique by their ID)
  final uniqueUsers = UniqueList<User>((user) => user.id);

  // Add users
  uniqueUsers.add(User(1, 'Ahmad'));
  uniqueUsers.add(User(2, 'Nour'));

  // Attempt to add a duplicate user (will not be added)
  uniqueUsers.add(User(1, 'Ahmad')); // This won't be added as ID 1 already exists

  // Retrieve unmodifiable list of users
  log(uniqueUsers.items.toString()); // [User(id: 1, name: Alice), User(id: 2, name: Bob)]

  // Remove a user - ⚠️ NOTE: This may not work as expected
  // ⚠️ Won't work unless you override `==` and `hashCode` for User
  uniqueUsers.remove(User(2, 'Nour'));

  // Explanation:
  // The `remove` method uses the `==` operator to check if the items match. 
  // By default, Dart compares objects by their memory reference, not by the fields inside the object.
  // To make this work, you need to either manually override the `==` operator and `hashCode` 
  // for your model (as shown above), or use third-party libraries like `equatable`, `freezed`, etc.

  // As an alternative, you can use `removeOneWhere` to remove an item by a condition
  uniqueUsers.removeOneWhere((user) => user.id == 2); // Works even without `==` override

  // Check if list contains a user
  bool exists = uniqueUsers.contains(User(1, 'Ahmad')); // true
}
```