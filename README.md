# Pipeline-Oriented-Programming



## Examples

- 목표 : 주어진 리스트를 (MinMaxScaling / StandardScaling) 하는 파이프라인 구현

### Function Based POP
- 가장 초기에 POP를 하기 위해 진행한 코드 스타일
- 특징
    - 개발을 진행하기 쉽다.
    - 평균적인 코드 길이자체는 가장 짧다
    - 최종적으로 원하는 함수가 무엇인지 파악하기 힘들다.

```
# MinMaxScaling

def get_min_value(numeric_list):
    min_value = min(numeric_list)
    return min_value

def get_max_value(numeric_list):
    max_value = max_value(numeric_list)
    return max_value

def get_mms_value(value, min_value, max_value):
    mms_value = (value - min_value) / (max_value - min_value)
    return mms_value

def get_mms_list(numeric_list):
    min_value = get_min_value(numeric_list)
    max_value = get_max_value(numeric_list)
    mms_list = [get_mms_value(value, min_value, max_value) for value in numeric_list]
    return mms_list

mms_list = get_mms_list(numeric_list)

---
# StandardScaling

def get_mean(numeric_list):
    mean = sum(numeric_list) / len(numeric_list)
    return mean

def get_variance(numeric_list):
    mean = get_mean(numeric_list)
    variance = sum([((value - mean) ** 2 / (len(numeric_list) - 1)) for value in numeric_list])
    return variance

def get_ssc_list(numeric_list):
    mean = get_mean(numeric_list)
    variance = get_variance(numeric_list)
    std = variance**0.5
    ssc_list = [(value - mean) / std for value in numeric_list]
    return ssc_list

ssc_list = get_ssc_list(numeric_list)
--
```

### Class Based POP

#### General Class Based POP
- 함수형 POP 보완을 위해 시작한 코드스타일
- 특징
    - 함수형의 "최종적으로 원하는 함수가 무엇인지 파악하기 힘들다." 부분 보완
    - 에러 처리가 유용하며, 하나의 파이프라인이 어떤 직무를 가지는지 명확하다.
```
# MinMaxScaling
class MINMAXSCALER:
    def __init__(self, numeric_list):
        self.numeric_list = numeric_list

    def get_min_value(self):
        min_value = min(self.numeric_list)
        return min_value

    def get_max_value(self):
        max_value = max(self.numeric_list)
        return max_value

    def get_mms_value(self, value):
        min_value = self.get_min_value()
        max_value = self.get_max_value()
        mms_value = (value - min_value) / (max_value - min_value)
        return mms_value

    def get_mms_list(self):
        numeric_list = self.numeric_list
        mms_list = [self.get_mms_value(value) for value in numeric_list]
        return mms_list


minmaxscaler = MINMAXSCALER([1, 2, 3, 4, 5])
mms_list = minmaxscaler.get_mms_list()

# StandardScaling
class STANDARDSCALER:
    def __init__(self, numeric_list):
        self.numeric_list = numeric_list

    def get_mean(self):
        numeric_list = self.numeric_list
        mean = sum(numeric_list) / len(numeric_list)
        return mean

    def get_variance(self):
        numeric_list = self.numeric_list
        mean = self.get_mean()
        variance = sum([((value - mean) ** 2 / (len(numeric_list) - 1)) for value in numeric_list])
        return variance

    def get_ssc_list(self):
        numeric_list = self.numeric_list
        mean = self.get_mean()
        variance = self.get_variance()
        std = variance**0.5
        ssc_list = [(value - mean) / std for value in numeric_list]
        return ssc_list


standardscaler = STANDARDSCALER([1, 2, 3, 4, 5])
ssc_list = standardscaler.get_ssc_list()
```

#### Inner-Class Based POP
- 
- 하지만 클래스 내부의 자원을 다른 곳에서 사용하기 힘들어 DRY 철학을 위배하기 쉽다.
```
class SCALER:
    class MINMAXSCALER:
        def __init__(self, numeric_list):
            self.numeric_list = numeric_list

        def get_min_value(self):
            min_value = min(self.numeric_list)
            return min_value

        def get_max_value(self):
            max_value = max(self.numeric_list)
            return max_value

        def get_mms_value(self, value):
            min_value = self.get_min_value()
            max_value = self.get_max_value()
            mms_value = (value - min_value) / (max_value - min_value)
            return mms_value

        def get_mms_list(self):
            numeric_list = self.numeric_list
            mms_list = [self.get_mms_value(value) for value in numeric_list]
            return mms_list

    class STANDARDSCALER:
        def __init__(self, numeric_list):
            self.numeric_list = numeric_list

        def get_mean(self):
            numeric_list = self.numeric_list
            mean = sum(numeric_list) / len(numeric_list)
            return mean

        def get_variance(self):
            numeric_list = self.numeric_list
            mean = self.get_mean()
            variance = sum(
                [((value - mean) ** 2 / (len(numeric_list) - 1)) for value in numeric_list]
            )
            return variance

        def get_ssc_list(self):
            numeric_list = self.numeric_list
            mean = self.get_mean()
            variance = self.get_variance()
            std = variance**0.5
            ssc_list = [(value - mean) / std for value in numeric_list]
            return ssc_list

mms = SCALER.MINMAXSCALER([1, 2, 3, 4, 5])
mms.get_mms_list()

ssc = SCALER.STANDARDSCALER([1, 2, 3, 4, 5])
ssc.get_ssc_list()

```

#### Private-Method Based POP

```
class SCALER:
    def get_mms_value():
        pass

    def get_ssc_value():
        pass

    @staticmethod
    def _get_min_value():
        pass

    @staticmethod
    def _get_max_value():
        pass
```

