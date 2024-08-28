This Python function, `get_logger`, is designed to create and configure a logger specifically for handling user data. This logger will ensure that sensitive information (PII—Personally Identifiable Information) in the logs is redacted before being output. Let's break down the function step by step:

### 1. **Importing Required Modules**

Before you use this function, you need to have the `logging` module imported, which is a standard Python module for logging.

### 2. **Function Overview**
```python
def get_logger() -> logging.Logger:
    """Creates a new logger for user data."""
```
- **`get_logger`**: This function returns a `logging.Logger` instance, which is a configured logger tailored for user data.
- **Return Type**: The function returns a `logging.Logger` object, indicated by the return type hint `-> logging.Logger`.

### 3. **Creating a Logger**
```python
logger = logging.getLogger("user_data")
```
- **`logging.getLogger("user_data")`**: This line retrieves or creates a logger named `"user_data"`. Loggers are named entities in Python's logging system, and using a specific name allows you to configure loggers for different parts of your application separately.

### 4. **Creating a StreamHandler**
```python
stream_handler = logging.StreamHandler()
```
- **`logging.StreamHandler()`**: This creates a `StreamHandler` instance, which sends logging output to streams like `sys.stdout` or `sys.stderr`. By default, it sends the output to `sys.stderr`.

### 5. **Setting a Formatter**
```python
stream_handler.setFormatter(RedactingFormatter(PII_FIELDS))
```
- **`RedactingFormatter(PII_FIELDS)`**: `RedactingFormatter` is a custom formatter (presumably defined elsewhere in your code) that takes `PII_FIELDS` as an argument. It is responsible for formatting log messages and redacting sensitive information based on the fields specified in `PII_FIELDS`.
- **`setFormatter`**: This method sets the formatter for the `StreamHandler`, ensuring that every log message processed by this handler will be formatted (and redacted) according to the rules defined in `RedactingFormatter`.

### 6. **Setting the Log Level**
```python
logger.setLevel(logging.INFO)
```
- **`setLevel(logging.INFO)`**: This sets the logging level to `INFO`. The logger will process and output messages at the `INFO` level and higher (e.g., `WARNING`, `ERROR`, `CRITICAL`), but it will ignore lower-level messages like `DEBUG`.

### 7. **Disabling Propagation**
```python
logger.propagate = False
```
- **`logger.propagate = False`**: By setting `propagate` to `False`, you prevent log messages from being passed to higher-level loggers in the logging hierarchy. This is useful if you want the messages logged by `"user_data"` to only appear in the configured stream handler and not be duplicated in other handlers.

### 8. **Adding the Handler to the Logger**
```python
logger.addHandler(stream_handler)
```
- **`addHandler(stream_handler)`**: This attaches the `StreamHandler` to the logger. Now, the logger will send its output through this handler, which means it will be formatted and redacted before being displayed.

### 9. **Returning the Logger**
```python
return logger
```
- Finally, the function returns the configured `logger` instance.

### Example Usage

Once you have the `get_logger` function defined, you can use the logger in your application like this:

```python
logger = get_logger()
logger.info("User logged in with email=johndoe@example.com and password=secret")
```

If the `RedactingFormatter` works correctly, the output would be something like:

```
User logged in with email=[REDACTED] and password=[REDACTED]
```

This ensures that any sensitive information in the logs is appropriately redacted before being output to the console or any other configured log destination.