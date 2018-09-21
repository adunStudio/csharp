# String.Format() vs. Concatenation vs. String Builder

C# 에서 문자열을 더해서 만드는 3가지 방법

- String 클래스에 내장된 `string.Format` 메서드
- 언어 자체 기능인 Concatenation `("a" + "b")`
- `System.Text.StringBuilder` 클래스

위 세가지 중 무엇이 가장 빠를까?

---

## Study & Example

### string.Format

```c#
// Placeholders are of the form: {index}
string.Format("{0} + {1} = {2}", 2, 3, 5); // returns "2 + 3 = 5"
 
// Mixed types are OK, too
string.Format("{0} {1}, {2}", "August", 5, 1972); // returns "August 5, 1972"
```

### Concatenation

```c#
// Strings can be concatenated by the + operator
"Hello," + " " + "world!"; // evaluates to: "Hello, world!"
 
// Mixed types are OK, too
"August " + 5 + ", " + 1972; // evaluates to: "August 5, 1972"
```

### System.Text.StringBuilder

```c#
var builder = new StringBuilder(); // initially empty
var builder = new StringBuilder(1024); // empty, but with 1024 bytes of capacity
var builder = new StringBuilder("hello"); // initially "hello"
 
// Append various types
builder.Append("string");
builder.Append('c');
builder.Append(123);
builder.Append(3.1415);
 
// Get the result string
var str = builder.ToString();
```

---

## Test

위 각각의 방법으로 1000개의 단일 문자열로 만드는 테스트를 100000번씩 반복하여 테스트를 진행해봤다.

```c#
using UnityEngine;
using System;
using System.Text;
using System.Diagnostics;


public static class StopWatchExtensions
{
    public static long RunTest(this Stopwatch stopWatch, int iterationCount, Action testFunction)
    {
        stopWatch.Reset();
        stopWatch.Start();

        for (int i = 0; i < iterationCount; ++i)
        {
            testFunction();
        }

        return stopWatch.ElapsedMilliseconds;
    }
}

public class StringTest : MonoBehaviour
{
    private StringBuilder builder = new StringBuilder(1024);
    private string report;

    void Start()
    {
        var iterationCount = 100000;
        var formatString = "";

        for (int i = 0; i < 1000; ++i)
        {
            formatString += "{" + i + "}";
        }

        var stopWatch = new Stopwatch();

        var stringFormatTime = stopWatch.RunTest(iterationCount, ()=> StringFormat(formatString));

        var concatenationTime = stopWatch.RunTest(iterationCount, ()=> Concatenation());

        var stringBuilderTime = stopWatch.RunTest(iterationCount, ()=> StringBuilder());

        report = 
            "Test: result\n"   +
            "String.Format: "  + stringFormatTime  + "\n" +
            "Concatenation: "  + concatenationTime + "\n" +
            "StringBuilder: "  + stringBuilderTime;
    }

    void OnGUI()
    {
        GUI.TextArea(new Rect(0, 0, Screen.width, Screen.height), report);
    }

    private string StringFormat(string formatString)
    {
        var s = "A";

        return string.Format(
            formatString,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s,
            s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s, s
        );
    }

    private string Concatenation()
    {
        var s = "A";

        return
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s +
            s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s + s;
    }

    private string StringBuilder()
    {
        var s = "A";

        var builder = new StringBuilder();

        for (int i = 0; i < 1000; ++i)
        {
            builder.Append(s);
        }

        return builder.ToString();
    }
}
```



>Test: result
>
>String.Format: 7843
>
>Concatenation: 2099
>
>StringBuilder:  2260

---

# Result

- string.Format은 위 테스트에서 다른 방법들보다 3배정도 시간이 더 걸리며 가장 느리다.

- Concatenation과 StringBuilder는 성능차이가 크게나지 않는다.

하지만 StringBuilder에 추가적인 최적화방법이 있다. 위 테스트에서 StringBuilder의 인스턴스는 비어있는 상태로 시작했지만, 전체 문자열을 유지할 수 있을만큼 큰 초기용량이 주어지면 성능은 더 향상될 것이다.

```c#
var builder = new StringBuilder(1024);
```

마지막으로 매 반복문에서 StringBuilder의 인스턴스를 만드는대신, 하나의 StringBuilder 인스턴스의 길이를 초기화하는 방식으로 사용함으로써 성능향상을 할 수 있다.

```c#
private StringBuilder m_builder = new StringBuilder(1024);

private string StringBuilder()
{
	var s = "A";

    m_bulder.Length = 0;
    
	for (int i = 0; i < 1000; ++i)
	{
		m_builder.Append(s);
	}

	return m_builder.ToString();
}
```

> Test: result
>
> String.Format: 7959
>
> Concatenation: 2074
>
> StringBuilder:  1793

번역: https://jacksondunstan.com/articles/3015 