## 2.3 패키지

각 모듈은 완전한 경로를 통해 불러옵니다.

### 2.3.1 장점

모듈 검색 경로가 작성자가 예상한 것과 다를 경우 발생할 수 있는 모듈 이름의 충돌이나 잘못 불러오기를 방지합니다.
또한, 모듈을 더 쉽게 찾을 수 있도록 합니다.

### 2.3.2 단점

패키지 계층 구조를 유지해야 하므로 코드 배포가 복잡해질 수 있습니다.
그러나 최근 배포 시스템에서 패키지 구조의 유지가 일반적이므로 큰 문제는 되지 않습니다.

### 2.3.3 권장 사항

모든 새로운 코드는 전체 패키지 경로를 통해 각 모듈을 불러와야 합니다.
모듈의 임포트 방식은 다음과 같아야 합니다:

```python

# Yes(권장):
# 코드에서 absl.flags를 전체 이름(자세한 방식)으로 참조합니다.

import absl.flags
from doctor.who import jodie

_FOO = absl.flags.DEFINE_string(...)
```

```python

# Yes(권장):
# 일반적으로 코드에서 플래그를 참조할 때 모듈 이름만 사용합니다.

from absl import flags
from doctor.who import jodie

_FOO = flags.DEFINE_string(...)
```

(이 파일은 `doctor/who/` 디렉터리에 있으며, `jodie.py` 파일도 같은 위치에 있다고 가정합니다.)

```python

# No(주의):

# 작성자가 어떤 모듈을 의도했는지, 그리고 실제로 어떤 모듈이 임포트될 것인지 불분명합니다.
# 실제 임포트 방식은 sys.path를 제어하는 외부 요소에 따라 달라집니다.
# 작성자가 의도한 jodie 모듈은 무엇인지 알기 어렵습니다.

import jodie
```

메인 바이너리가 위치한 디렉터리가 `sys.path`에 포함된다고 가정해서는 안 됩니다.
일부 환경에서는 그렇게 동작할 수 있지만, 항상 보장되는 것은 아닙니다.
따라서, `import jodie`라는 코드가 로컬의 `jodie.py` 파일이 아니라, `jodie`라는 이름의 서드파티 패키지나 최상위 패키지로 인식될 수 있다고 가정해야 합니다.
