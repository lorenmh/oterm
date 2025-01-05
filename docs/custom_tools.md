# Custom tools with oterm

You can create your own custom tools and integrate them with `oterm`.

### Create a python package.

You will need to create a python package that exports a `Tool` definition as well as a *callable* function that will be called when the tool is invoked.

Here is an [example](./oracle/tool.py) of a simple tool that implements an Oracle. The tool is defined in the `oracle` package which exports the `OracleTool` tool definition and an `oracle` callable function.

```python
from ollama._types import Tool

OracleTool = Tool(
    type="function",
    function=Tool.Function(
        name="oracle",
        description="Function to return the Oracle's answer to any question.",
        parameters=Tool.Function.Parameters(
            type="object",
            properties={
                "question": Tool.Function.Parameters.Property(
                    type="str", description="The question to ask."
                ),
            },
            required=["question"],
        ),
    ),
)


def oracle(question: str):
    return "oterm"
```

You need to install the package in the same environment where `oterm` is installed so that `oterm` can resolve it.

```bash
cd oracle
uv pip install . # or pip install .
```

### Register the tool with oterm

You can register the tool with `oterm` by adding the tool definittion and callable to the `tools` section of the `oterm` configuration file. You can find the location of the configuration file's directory by running `oterm --data-dir`.

```json
{
    ...
    "tools": [{
        "tool": "oracle.tool:OracleTool",
        "callable": "oracle.tool:oracle"
    }]
}
```
Note the notation `module:object` for the tool and callable. 

That's it! You can now use the tool in `oterm` with models that support it.