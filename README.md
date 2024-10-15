# Markdown_bounty

# try to modify this for every chunk 

```dart
import 'package:flutter/material.dart';
import 'package:flutter_markdown_selectionarea/flutter_markdown_selectionarea.dart';

const markdownChunks = [
  '''
### Problem

We’re building an LLM based tool for one of our FilledStacks clients.
  ''',
  '''
As with ChatGPT, the response from the LLM is streamed back to us.
  ''',
  '''
The text comes back as it
  ''',
  '''
is being completed.
  ''',
  '''
Here’s an example of how
  ''',
  '''
paragraph would be returned:
  ''',
  '''
**The full paragraph**

“I need every new
  ''',
  '''
word being added to the text to animate i''',
  '''n using a fade functionality. This an
  ''',
  '''
example of this can be seen when using Gemini chat.”
  ''',
  '''
**How it’s returned**

“I need”
  ''',
  '''
“I need every new word”
  ''',
  '''
“I need every new word
  ''',
  '''
being added to”
  ''',
  '''
“I need every new word being
  ''',
  '''
added to the text”
  ''',
  '''
“I need every new word being added to the text to animate in”
  ''',
];

const defaultMessage = 'Tap FAB to add markdown';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _opacityAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 5000),
    );
    _opacityAnimation = Tween<double>(begin: 0.0, end: 1.0).animate(
      CurvedAnimation(parent: _controller, curve: Curves.easeIn),
    );

    // Start the fade-in animation as soon as the widget is built
    _controller.forward();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  String _currentMarkdown = defaultMessage;
  int _markdownIndex = 0;

  void _addMarkdown() {
    if (_markdownIndex < markdownChunks.length) {
      setState(() {
        if (_currentMarkdown == defaultMessage) {
          _currentMarkdown = markdownChunks[_markdownIndex];
        } else {
          _currentMarkdown += markdownChunks[_markdownIndex];
        }

        _markdownIndex++;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: ConstrainedBox(
          constraints: const BoxConstraints(maxWidth: 700),
          child: FadeTransition(
            opacity: _opacityAnimation, // Control opacity for fade-in
            // 1-second fade-in duration
            child: MarkdownBody(
              data: _currentMarkdown,
            ),
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _addMarkdown,
        tooltip: 'Add Markdown',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```
