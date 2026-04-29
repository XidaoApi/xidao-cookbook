# Migrate from OpenAI to XiDao API

If your app already uses the OpenAI SDK, migration is usually simple:

1. replace the API key
2. replace the base URL with `https://api.xidao.online/v1`
3. update the model name if needed

## Python example

```python
from openai import OpenAI
import os

client = OpenAI(
    api_key=os.environ["XIDAO_API_KEY"],
    base_url="https://api.xidao.online/v1"
)
```

## Why this works

XiDao API is designed for OpenAI-compatible integration, which reduces engineering cost for migration.
