## GPT-OSS - A C# Guide with Ollama 

Working with large language models is an exciting field, and running them locally opens up new possibilities for experimentation,  cost-efficiency, and privacy. In this guide, you’ll learn how to use the open-source GPT-OSS model with Ollama to build  build fast, private, and offline-capable AI features.

### What You'll Need
- A machine with at least 16 GB of RAM and a capable GPU (or an Apple Silicon Mac).

- The .NET 8 SDK or higher installed.

- Ollama installed and running.

 - The GPT-OSS:20b model pulled with `ollama pull gpt-oss:20b`.

   ## C\# Toolbox

Microsoft has made it incredibly easy to work with AI models using the **Microsoft.Extensions.AI** libraries. These libraries provide a unified set of abstractions, letting you write code that can work with different AI providers—like Ollama, Azure AI, or OpenAI—without changing your core logic.

### Step 1: Create a New Console App

First, let's create a new console application. Open your terminal or command prompt and run:

```bash
dotnet new console -n OllamaGPTOSS
cd OllamaGPTOSS
```

### Step 2: Add the NuGet Packages

To connect to Ollama using **Microsoft.Extensions.AI**, you'll need two main packages. The `Microsoft.Extensions.AI` package provides the core abstractions, while the `OllamaSharp` package acts as the provider that implements these abstractions for Ollama.

```bash
dotnet add package Microsoft.Extensions.AI
dotnet add package OllamaSharp
```

**Note:** The `Microsoft.Extensions.AI.Ollama` package is deprecated, and `OllamaSharp` is the recommended alternative for connecting to Ollama.

### Step 3: Write Your Chat Code

Now, open your `Program.cs` file and replace its contents with the following code. This example uses .NET's built-in dependency injection to configure and use the chat client.

```csharp
using Microsoft.Extensions.AI;
using OllamaSharp;

// Initialize OllamaApiClient targeting the "gpt-oss" model
IChatClient chatClient = new OllamaApiClient(new Uri("http://localhost:11434/"), "gpt-oss");

// Maintain conversation history
List<ChatMessage> chatHistory = new();

Console.WriteLine("GPT-OSS Chat - Type 'exit' to quit");
Console.WriteLine();

// Prompt user for input in a loop
while (true)
{
    Console.Write("You: ");
    var userInput = Console.ReadLine();
    
    if (userInput?.ToLower() == "exit")
        break;
    
    if (string.IsNullOrWhiteSpace(userInput))
        continue;

    // Add user message to chat history
    chatHistory.Add(new ChatMessage(ChatRole.User, userInput));

    // Stream the AI response and display in real time
    Console.Write("Assistant: ");
    var assistantResponse = "";
    
    await foreach (var update in chatClient.GetStreamingResponseAsync(chatHistory))
    {
        Console.Write(update.Text);
        assistantResponse += update.Text;
    }
    
    // Append assistant message to chat history
    chatHistory.Add(new ChatMessage(ChatRole.Assistant, assistantResponse));
    Console.WriteLine();
    Console.WriteLine();
}
```

### Step 4: Run Your Application\!

Make sure your Ollama service is running in the background. Then, run your .NET console app:

```bash
dotnet run
```

![Running GPT-OSS Application](./images/running-app.png "GPT-OSS console application in action")

Your application will connect to the local Ollama server, and you can start chatting with your own private GPT-OSS model.

-----

## Call to Action: Your Code, Your AI 🛠️

This is just the beginning. The **Microsoft.Extensions.AI** libraries also support **function calling**, allowing you to give your local LLM access to your C\# methods, APIs, and data. This is where you can build truly powerful, "agentic" applications.

**Your mission, if you choose to accept it:**

1.  **Get this sample running** and prove to yourself that local LLM development is easy.
2.  **Explore the documentation** for `Microsoft.Extensions.AI` and `OllamaSharp`.
3.  **Integrate this into a project.** Build a local document summarizer, a code generator, or an intelligent assistant that runs entirely on your own terms.

The future of AI is decentralized, and as a C\# developer, you have the tools to lead the charge. The power is on your machine—now go build something incredible\!
