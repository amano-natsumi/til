# Test
## assert datetime

```python
from datetime import timedelta
from django.utils import timezone
from django.test import TestCase

class Test(TestCase):
    def assertAboutOver1hour(self, value):
        assert isinstance(value, datetime)
        delta = timezone.localtime(value) - (timezone.now() + timedelta(seconds=60*60))
        assert timedelta(seconds=-5) < delta < timedelta(seconds=5)

```
