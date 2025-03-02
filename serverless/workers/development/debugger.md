# Debugging

In the previous lesson, we covered the basics of running your SubModel serverless functions locally. Now, let's delve into some advanced options that provide greater control over your local testing environment.

The SubModel Python SDK offers a variety of command-line arguments that allow you to customize your local testing setup. Let's explore each of these options in detail:

## Controlling Log Levels

```bash
--rp_log_level ERROR | WARN | INFO | DEBUG
```

This argument allows you to set the verbosity of the console output. The available options are:

- `ERROR`: Displays only error messages
- `WARN`: Displays warnings and errors
- `INFO`: Displays general information, warnings, and errors
- `DEBUG`: Displays all messages, including detailed debug information

Example:

```bash
python hello_world.py --rp_server_api --rp_log_level DEBUG
```

The `--rp_log_level` flag activates the SubModel debugger, which can assist you in troubleshooting issues within your code.

Example:

```bash
python hello_world.py --rp_server_api --rp_debugger
```

## Customizing the API Server

The following arguments allow you to configure the FastAPI server that simulates the SubModel serverless environment:

```bash
--rp_serve_api
--rp_api_port <port_number>
--rp_api_concurrency <number_of_workers>
--rp_api_host <hostname>
```

- `--rp_serve_api`: Starts the API server
- `--rp_api_port`: Sets the port number (default is 8000)
- `--rp_api_concurrency`: Sets the number of concurrent workers (default is 1)
- `--rp_api_host`: Sets the hostname (default is "localhost")

Example:

```bash
python hello_world.py --rp_serve_api --rp_api_port 8080 --rp_api_concurrency 4 --rp_api_host 0.0.0.0
```

This command starts the API server on port 8080 with 4 concurrent workers and makes it accessible from other devices on the network.

## Providing Test Input

As we saw in the previous lesson, you can provide test input either through a JSON file or directly via the command line:

```bash
--test_input '<JSON_string>'
```

Example:

```bash
python hello_world.py --rp_server_api --test_input '{"input": {"name": "SubModel"}}'
```

You can combine these arguments to create a highly customized local testing environment. Here's an example that uses multiple options:

```bash
python hello_world.py --rp_server_api --rp_log_level DEBUG --rp_debugger --rp_api_port 8080 --rp_api_concurrency 2 --test_input '{"input": {"name": "Advanced Tester"}}'
```

This command:

1. Starts the local server
2. Sets the log level to DEBUG for maximum information
3. Enables the debugger
4. Uses port 8080 for the API server
5. Sets up 2 concurrent workers
6. Provides a test input directly in the command

## Conclusion

These advanced options for local testing with the SubModel Python SDK provide you with fine-grained control over your development environment. By mastering these tools, you can ensure your serverless functions are robust and ready for deployment to the SubModel cloud.

In the next lesson, we'll explore how to structure more complex handlers to tackle advanced use cases in your serverless applications.