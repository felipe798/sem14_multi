# sem14_multi

```dart

import 'package:flutter/material.dart';
import 'package:flutter/cupertino.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return CupertinoApp(
      title: 'Tareas App',
      theme: CupertinoThemeData(
        primaryColor: CupertinoColors.systemBlue,
        brightness: Brightness.light,
      ),
      home: LoginScreen(),
    );
  }
}

// Modelo de Usuario
class User {
  final String name;
  final String email;
  final DateTime birthDate;

  User({required this.name, required this.email, required this.birthDate});
}

// Modelo de Tarea
class Task {
  final String title;
  final String description;
  final DateTime createdDate;
  final DateTime dueDate;
  bool isCompleted;

  Task({
    required this.title,
    required this.description,
    required this.createdDate,
    required this.dueDate,
    this.isCompleted = false,
  });
}

// Pantalla de Login
class LoginScreen extends StatefulWidget {
  @override
  _LoginScreenState createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final TextEditingController _emailController = TextEditingController();
  final TextEditingController _passwordController = TextEditingController();

  void _login() {
    if (_emailController.text.isNotEmpty && _passwordController.text.isNotEmpty) {
      Navigator.of(context).pushReplacement(
        CupertinoPageRoute(builder: (context) => MenuScreen()),
      );
    } else {
      _showAlert('Error', 'Por favor ingresa email y contraseña');
    }
  }

  void _showAlert(String title, String message) {
    showCupertinoDialog(
      context: context,
      builder: (context) => CupertinoAlertDialog(
        title: Text(title),
        content: Text(message),
        actions: [
          CupertinoDialogAction(
            child: Text('OK'),
            onPressed: () => Navigator.pop(context),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: CupertinoNavigationBar(
        middle: Text('Iniciar Sesión'),
        backgroundColor: CupertinoColors.systemBackground,
      ),
      child: SafeArea(
        child: Padding(
          padding: EdgeInsets.all(20),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Icon(
                CupertinoIcons.person_circle_fill,
                size: 120,
                color: CupertinoColors.systemBlue,
              ),
              SizedBox(height: 40),
              CupertinoTextField(
                controller: _emailController,
                placeholder: 'Email',
                keyboardType: TextInputType.emailAddress,
                prefix: Padding(
                  padding: EdgeInsets.only(left: 8),
                  child: Icon(CupertinoIcons.mail, color: CupertinoColors.systemGrey),
                ),
                padding: EdgeInsets.all(16),
              ),
              SizedBox(height: 16),
              CupertinoTextField(
                controller: _passwordController,
                placeholder: 'Contraseña',
                obscureText: true,
                prefix: Padding(
                  padding: EdgeInsets.only(left: 8),
                  child: Icon(CupertinoIcons.lock, color: CupertinoColors.systemGrey),
                ),
                padding: EdgeInsets.all(16),
              ),
              SizedBox(height: 30),
              SizedBox(
                width: double.infinity,
                child: CupertinoButton.filled(
                  child: Text('Iniciar Sesión'),
                  onPressed: _login,
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// Pantalla de Menú Principal
class MenuScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: CupertinoNavigationBar(
        middle: Text('Menú Principal'),
        backgroundColor: CupertinoColors.systemBackground,
        trailing: CupertinoButton(
          padding: EdgeInsets.zero,
          child: Icon(CupertinoIcons.power),
          onPressed: () {
            Navigator.of(context).pushReplacement(
              CupertinoPageRoute(builder: (context) => LoginScreen()),
            );
          },
        ),
      ),
      child: SafeArea(
        child: Padding(
          padding: EdgeInsets.all(16),
          child: Column(
            children: [
              SizedBox(height: 20),
              _buildMenuCard(
                context,
                'Registro de Usuario',
                'Registra un nuevo usuario',
                CupertinoIcons.person_add,
                () => Navigator.push(
                  context,
                  CupertinoPageRoute(builder: (context) => UserRegistrationScreen()),
                ),
              ),
              SizedBox(height: 16),
              _buildMenuCard(
                context,
                'Registro de Tareas',
                'Crea una nueva tarea',
                CupertinoIcons.plus_circle,
                () => Navigator.push(
                  context,
                  CupertinoPageRoute(builder: (context) => TaskRegistrationScreen()),
                ),
              ),
              SizedBox(height: 16),
              _buildMenuCard(
                context,
                'Lista de Tareas',
                'Ver todas las tareas',
                CupertinoIcons.list_bullet,
                () => Navigator.push(
                  context,
                  CupertinoPageRoute(builder: (context) => TaskListScreen()),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildMenuCard(BuildContext context, String title, String subtitle, IconData icon, VoidCallback onTap) {
    return Container(
      width: double.infinity,
      child: CupertinoButton(
        padding: EdgeInsets.zero,
        onPressed: onTap,
        child: Container(
          padding: EdgeInsets.all(20),
          decoration: BoxDecoration(
            color: CupertinoColors.systemBackground,
            borderRadius: BorderRadius.circular(12),
            border: Border.all(color: CupertinoColors.systemGrey4),
          ),
          child: Row(
            children: [
              Container(
                padding: EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: CupertinoColors.systemBlue,
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Icon(icon, color: CupertinoColors.white, size: 24),
              ),
              SizedBox(width: 16),
              Expanded(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      title,
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.w600,
                        color: CupertinoColors.label,
                      ),
                    ),
                    SizedBox(height: 4),
                    Text(
                      subtitle,
                      style: TextStyle(
                        fontSize: 14,
                        color: CupertinoColors.secondaryLabel,
                      ),
                    ),
                  ],
                ),
              ),
              Icon(CupertinoIcons.chevron_right, color: CupertinoColors.systemGrey),
            ],
          ),
        ),
      ),
    );
  }
}

// Pantalla de Registro de Usuario
class UserRegistrationScreen extends StatefulWidget {
  @override
  _UserRegistrationScreenState createState() => _UserRegistrationScreenState();
}

class _UserRegistrationScreenState extends State<UserRegistrationScreen> {
  final TextEditingController _nameController = TextEditingController();
  final TextEditingController _emailController = TextEditingController();
  DateTime _birthDate = DateTime.now();

  void _showDatePicker() {
    showCupertinoModalPopup(
      context: context,
      builder: (context) => Container(
        height: 300,
        color: CupertinoColors.systemBackground,
        child: Column(
          children: [
            Container(
              height: 50,
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  CupertinoButton(
                    child: Text('Cancelar'),
                    onPressed: () => Navigator.pop(context),
                  ),
                  CupertinoButton(
                    child: Text('Confirmar'),
                    onPressed: () => Navigator.pop(context),
                  ),
                ],
              ),
            ),
            Expanded(
              child: CupertinoDatePicker(
                mode: CupertinoDatePickerMode.date,
                initialDateTime: _birthDate,
                maximumDate: DateTime.now(),
                onDateTimeChanged: (date) {
                  setState(() {
                    _birthDate = date;
                  });
                },
              ),
            ),
          ],
        ),
      ),
    );
  }

  void _registerUser() {
    if (_nameController.text.isNotEmpty && _emailController.text.isNotEmpty) {
      showCupertinoDialog(
        context: context,
        builder: (context) => CupertinoAlertDialog(
          title: Text('Éxito'),
          content: Text('Usuario registrado correctamente'),
          actions: [
            CupertinoDialogAction(
              child: Text('OK'),
              onPressed: () {
                Navigator.pop(context);
                Navigator.pop(context);
              },
            ),
          ],
        ),
      );
    } else {
      showCupertinoDialog(
        context: context,
        builder: (context) => CupertinoAlertDialog(
          title: Text('Error'),
          content: Text('Por favor completa todos los campos'),
          actions: [
            CupertinoDialogAction(
              child: Text('OK'),
              onPressed: () => Navigator.pop(context),
            ),
          ],
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: CupertinoNavigationBar(
        middle: Text('Registro de Usuario'),
        backgroundColor: CupertinoColors.systemBackground,
      ),
      child: SafeArea(
        child: Padding(
          padding: EdgeInsets.all(20),
          child: Column(
            children: [
              SizedBox(height: 20),
              CupertinoTextField(
                controller: _nameController,
                placeholder: 'Nombre completo',
                prefix: Padding(
                  padding: EdgeInsets.only(left: 8),
                  child: Icon(CupertinoIcons.person, color: CupertinoColors.systemGrey),
                ),
                padding: EdgeInsets.all(16),
              ),
              SizedBox(height: 16),
              CupertinoTextField(
                controller: _emailController,
                placeholder: 'Email',
                keyboardType: TextInputType.emailAddress,
                prefix: Padding(
                  padding: EdgeInsets.only(left: 8),
                  child: Icon(CupertinoIcons.mail, color: CupertinoColors.systemGrey),
                ),
                padding: EdgeInsets.all(16),
              ),
              SizedBox(height: 16),
              CupertinoButton(
                onPressed: _showDatePicker,
                child: Container(
                  width: double.infinity,
                  padding: EdgeInsets.all(16),
                  decoration: BoxDecoration(
                    color: CupertinoColors.systemBackground,
                    border: Border.all(color: CupertinoColors.systemGrey4),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: Row(
                    children: [
                      Icon(CupertinoIcons.calendar, color: CupertinoColors.systemGrey),
                      SizedBox(width: 8),
                      Text(
                        'Fecha de nacimiento: ${_birthDate.day}/${_birthDate.month}/${_birthDate.year}',
                        style: TextStyle(color: CupertinoColors.label),
                      ),
                    ],
                  ),
                ),
              ),
              SizedBox(height: 30),
              SizedBox(
                width: double.infinity,
                child: CupertinoButton.filled(
                  child: Text('Registrar Usuario'),
                  onPressed: _registerUser,
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// Pantalla de Registro de Tareas
class TaskRegistrationScreen extends StatefulWidget {
  @override
  _TaskRegistrationScreenState createState() => _TaskRegistrationScreenState();
}

class _TaskRegistrationScreenState extends State<TaskRegistrationScreen> {
  final TextEditingController _titleController = TextEditingController();
  final TextEditingController _descriptionController = TextEditingController();
  DateTime _dueDate = DateTime.now().add(Duration(days: 1));

  void _showDatePicker() {
    showCupertinoModalPopup(
      context: context,
      builder: (context) => Container(
        height: 300,
        color: CupertinoColors.systemBackground,
        child: Column(
          children: [
            Container(
              height: 50,
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  CupertinoButton(
                    child: Text('Cancelar'),
                    onPressed: () => Navigator.pop(context),
                  ),
                  CupertinoButton(
                    child: Text('Confirmar'),
                    onPressed: () => Navigator.pop(context),
                  ),
                ],
              ),
            ),
            Expanded(
              child: CupertinoDatePicker(
                mode: CupertinoDatePickerMode.date,
                initialDateTime: _dueDate,
                minimumDate: DateTime.now(),
                onDateTimeChanged: (date) {
                  setState(() {
                    _dueDate = date;
                  });
                },
              ),
            ),
          ],
        ),
      ),
    );
  }

  void _registerTask() {
    if (_titleController.text.isNotEmpty && _descriptionController.text.isNotEmpty) {
      TaskListScreen.tasks.add(Task(
        title: _titleController.text,
        description: _descriptionController.text,
        createdDate: DateTime.now(),
        dueDate: _dueDate,
      ));
      
      showCupertinoDialog(
        context: context,
        builder: (context) => CupertinoAlertDialog(
          title: Text('Éxito'),
          content: Text('Tarea registrada correctamente'),
          actions: [
            CupertinoDialogAction(
              child: Text('OK'),
              onPressed: () {
                Navigator.pop(context);
                Navigator.pop(context);
              },
            ),
          ],
        ),
      );
    } else {
      showCupertinoDialog(
        context: context,
        builder: (context) => CupertinoAlertDialog(
          title: Text('Error'),
          content: Text('Por favor completa todos los campos'),
          actions: [
            CupertinoDialogAction(
              child: Text('OK'),
              onPressed: () => Navigator.pop(context),
            ),
          ],
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: CupertinoNavigationBar(
        middle: Text('Registro de Tarea'),
        backgroundColor: CupertinoColors.systemBackground,
      ),
      child: SafeArea(
        child: Padding(
          padding: EdgeInsets.all(20),
          child: Column(
            children: [
              SizedBox(height: 20),
              CupertinoTextField(
                controller: _titleController,
                placeholder: 'Título de la tarea',
                prefix: Padding(
                  padding: EdgeInsets.only(left: 8),
                  child: Icon(CupertinoIcons.tag, color: CupertinoColors.systemGrey),
                ),
                padding: EdgeInsets.all(16),
              ),
              SizedBox(height: 16),
              CupertinoTextField(
                controller: _descriptionController,
                placeholder: 'Descripción',
                maxLines: 4,
                prefix: Padding(
                  padding: EdgeInsets.only(left: 8, top: 8),
                  child: Icon(CupertinoIcons.text_alignleft, color: CupertinoColors.systemGrey),
                ),
                padding: EdgeInsets.all(16),
              ),
              SizedBox(height: 16),
              CupertinoButton(
                onPressed: _showDatePicker,
                child: Container(
                  width: double.infinity,
                  padding: EdgeInsets.all(16),
                  decoration: BoxDecoration(
                    color: CupertinoColors.systemBackground,
                    border: Border.all(color: CupertinoColors.systemGrey4),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: Row(
                    children: [
                      Icon(CupertinoIcons.calendar, color: CupertinoColors.systemGrey),
                      SizedBox(width: 8),
                      Text(
                        'Fecha límite: ${_dueDate.day}/${_dueDate.month}/${_dueDate.year}',
                        style: TextStyle(color: CupertinoColors.label),
                      ),
                    ],
                  ),
                ),
              ),
              SizedBox(height: 30),
              SizedBox(
                width: double.infinity,
                child: CupertinoButton.filled(
                  child: Text('Registrar Tarea'),
                  onPressed: _registerTask,
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// Pantalla de Lista de Tareas
class TaskListScreen extends StatefulWidget {
  static List<Task> tasks = [
    Task(
      title: 'Ejemplo de tarea',
      description: 'Esta es una tarea de ejemplo',
      createdDate: DateTime.now().subtract(Duration(days: 1)),
      dueDate: DateTime.now().add(Duration(days: 2)),
    ),
  ];

  @override
  _TaskListScreenState createState() => _TaskListScreenState();
}

class _TaskListScreenState extends State<TaskListScreen> {
  void _toggleTaskCompletion(int index) {
    setState(() {
      TaskListScreen.tasks[index].isCompleted = !TaskListScreen.tasks[index].isCompleted;
    });
  }

  void _deleteTask(int index) {
    showCupertinoDialog(
      context: context,
      builder: (context) => CupertinoAlertDialog(
        title: Text('Eliminar Tarea'),
        content: Text('¿Estás seguro de que quieres eliminar esta tarea?'),
        actions: [
          CupertinoDialogAction(
            child: Text('Cancelar'),
            onPressed: () => Navigator.pop(context),
          ),
          CupertinoDialogAction(
            isDestructiveAction: true,
            child: Text('Eliminar'),
            onPressed: () {
              setState(() {
                TaskListScreen.tasks.removeAt(index);
              });
              Navigator.pop(context);
            },
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: CupertinoNavigationBar(
        middle: Text('Lista de Tareas'),
        backgroundColor: CupertinoColors.systemBackground,
        trailing: CupertinoButton(
          padding: EdgeInsets.zero,
          child: Icon(CupertinoIcons.add),
          onPressed: () => Navigator.push(
            context,
            CupertinoPageRoute(builder: (context) => TaskRegistrationScreen()),
          ),
        ),
      ),
      child: SafeArea(
        child: TaskListScreen.tasks.isEmpty
            ? Center(
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    Icon(
                      CupertinoIcons.list_bullet,
                      size: 80,
                      color: CupertinoColors.systemGrey,
                    ),
                    SizedBox(height: 16),
                    Text(
                      'No hay tareas registradas',
                      style: TextStyle(
                        fontSize: 18,
                        color: CupertinoColors.systemGrey,
                      ),
                    ),
                  ],
                ),
              )
            : ListView.builder(
                padding: EdgeInsets.all(16),
                itemCount: TaskListScreen.tasks.length,
                itemBuilder: (context, index) {
                  final task = TaskListScreen.tasks[index];
                  return Container(
                    margin: EdgeInsets.only(bottom: 12),
                    decoration: BoxDecoration(
                      color: CupertinoColors.systemBackground,
                      borderRadius: BorderRadius.circular(12),
                      border: Border.all(color: CupertinoColors.systemGrey4),
                    ),
                    child: CupertinoListTile(
                      leading: CupertinoButton(
                        padding: EdgeInsets.zero,
                        child: Icon(
                          task.isCompleted
                              ? CupertinoIcons.check_mark_circled_solid
                              : CupertinoIcons.circle,
                          color: task.isCompleted
                              ? CupertinoColors.systemGreen
                              : CupertinoColors.systemGrey,
                        ),
                        onPressed: () => _toggleTaskCompletion(index),
                      ),
                      title: Text(
                        task.title,
                        style: TextStyle(
                          decoration: task.isCompleted
                              ? TextDecoration.lineThrough
                              : TextDecoration.none,
                          color: task.isCompleted
                              ? CupertinoColors.systemGrey
                              : CupertinoColors.label,
                        ),
                      ),
                      subtitle: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            task.description,
                            style: TextStyle(
                              color: task.isCompleted
                                  ? CupertinoColors.systemGrey
                                  : CupertinoColors.secondaryLabel,
                            ),
                          ),
                          SizedBox(height: 4),
                          Text(
                            'Vence: ${task.dueDate.day}/${task.dueDate.month}/${task.dueDate.year}',
                            style: TextStyle(
                              fontSize: 12,
                              color: task.dueDate.isBefore(DateTime.now())
                                  ? CupertinoColors.systemRed
                                  : CupertinoColors.systemGrey,
                            ),
                          ),
                        ],
                      ),
                      trailing: CupertinoButton(
                        padding: EdgeInsets.zero,
                        child: Icon(
                          CupertinoIcons.delete,
                          color: CupertinoColors.systemRed,
                        ),
                        onPressed: () => _deleteTask(index),
                      ),
                    ),
                  );
                },
              ),
      ),
    );
  }
}

// Widget personalizado para CupertinoListTile (ya que no existe nativamente)
class CupertinoListTile extends StatelessWidget {
  final Widget? leading;
  final Widget title;
  final Widget? subtitle;
  final Widget? trailing;

  const CupertinoListTile({
    Key? key,
    this.leading,
    required this.title,
    this.subtitle,
    this.trailing,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: EdgeInsets.all(16),
      child: Row(
        children: [
          if (leading != null) ...[
            leading!,
            SizedBox(width: 12),
          ],
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                title,
                if (subtitle != null) ...[
                  SizedBox(height: 4),
                  subtitle!,
                ],
              ],
            ),
          ),
          if (trailing != null) ...[
            SizedBox(width: 12),
            trailing!,
          ],
        ],
      ),
    );
  }
}

```
