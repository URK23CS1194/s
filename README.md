
import 'dart:convert';
import 'package:flutter/material.dart';

// Employee Class
class Employee {
  int id;
  String name;
  String department;

  Employee(this.id, this.name, this.department);

  // Convert Employee into JSON Map
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'department': department,
    };
  }

  // Convert JSON Map into Employee
  factory Employee.fromJson(Map<String, dynamic> json) {
    return Employee(
      json['id'],
      json['name'],
      json['department'],
    );
  }
}

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: EmployeePage(),
    );
  }
}

class EmployeePage extends StatefulWidget {
  @override
  State<EmployeePage> createState() => _EmployeePageState();
}

class _EmployeePageState extends State<EmployeePage> {

  // Create Employee Object
  Employee emp = Employee(201, "Ravi", "IT");

  List<String> employees = [];

  @override
  Widget build(BuildContext context) {

    // Convert Employee to JSON
    String jsonString = jsonEncode(emp.toJson());

    // Convert JSON back to Employee
    Employee newEmp = Employee.fromJson(jsonDecode(jsonString));

    return Scaffold(
      appBar: AppBar(
        title: Text("Employee Record Manager"),
      ),

      body: Padding(
        padding: EdgeInsets.all(20),

        child: Column(
          children: [

            Text(
              "Employee Details: ID ${newEmp.id} | Name ${newEmp.name} | Department ${newEmp.department}",
            ),

            SizedBox(height: 20),

            ElevatedButton(
              onPressed: () {
                setState(() {
                  employees.add(emp.name);
                });
              },
              child: Text("Add Employee"),
            ),

            SizedBox(height: 20),

            Text("Employee List:"),

            Expanded(
              child: ListView.builder(
                itemCount: employees.length,
                itemBuilder: (context, index) {
                  return ListTile(
                    title: Text(employees[index]),
                  );
                },
              ),
            ),

            Text("JSON: $jsonString"),
          ],
        ),
      ),
    );
  }
}
