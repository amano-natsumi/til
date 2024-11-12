# Python コマンド

## pip
### 更新のあるものを一括 アップデート

```
pip list --outdated --format=freeze | awk -F "=" '{print $1}' | xargs pip install -U pip
```

## django
# secret_key 生成

```
python -c "from django.core.management.utils import get_random_secret_key;print(get_random_secret_key())"
```


# decorator

```python
from functools import wraps


def decorator(command_name):
    def _wrapper(func):
        @wraps(func)
        def _inner(*args, **kwargs):
            func(*args, **kwargs)

        return _inner

    return _wrapper
    
    
@decorator(1)
def test_func():
    pass

```

## ランダムなパスワード生成

```
import string
import secrets
alphabet = string.ascii_letters + string.digits
def make_pw(length):
    while True:
        password = ''.join(secrets.choice(alphabet) for i in range(length))
        if (any(c.islower() for c in password)
                and any(c.isupper() for c in password)
                and sum(c.isdigit() for c in password) >= 3):
            break
    return password

print(make_pw(10))
```
