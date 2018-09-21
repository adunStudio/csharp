# List.Capacity

[Capacity](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.capacity?view=netframework-4.7.2) is the number of elements that the [List](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=netframework-4.7.2) can store before resizing is required, whereas [Count](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.count?view=netframework-4.7.2) is the number of elements that are actually in the [List](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=netframework-4.7.2).

[Capacity](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.capacity?view=netframework-4.7.2) is always greater than or equal to [Count](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.count?view=netframework-4.7.2). If [Count](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.count?view=netframework-4.7.2) exceeds [Capacity](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.capacity?view=netframework-4.7.2) while adding elements, the capacity is increased by automatically reallocating the internal array before copying the old elements and adding the new elements.

If the capacity is significantly larger than the count and you want to reduce the memory used by the [List](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=netframework-4.7.2), you can decrease capacity by calling the [TrimExcess](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.trimexcess?view=netframework-4.7.2) method or by setting the [Capacity](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.capacity?view=netframework-4.7.2) property explicitly to a lower value. When the value of [Capacity](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.capacity?view=netframework-4.7.2)is set explicitly, the internal array is also reallocated to accommodate the specified capacity, and all the elements are copied.

Retrieving the value of this property is an O(1) operation; setting the property is an O(*n*) operation, where *n* is the new capacity.



<https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.capacity?redirectedfrom=MSDN&view=netframework-4.7.2#System_Collections_Generic_List_1_Capacity>



List<T>의 내부 구현은 LinkedList가 아닌 ArrayList

capacity 만큼 메모리를 미리 잡고있는다. 



생각해야할 점

- capacity를 넘어가면 재할당 후 복사하므로 비용이 크다. (반복문에서 빈번하게 일어날 경우...)
- capacity가 증가될 시 count에 따라 기하 급수적으로 커질 수 있다.
- capacity * T.Size 만큼 메모리를 잡고 있으므로 낭비가 크다. (게임에서는 중요!)



**List<T>에 삽입할 개수를 미리 안다면 capacity를 미리 설정하여 메모리를 확보하는 습관을 가지자.**



ex)

```c#
monsters.capacity = monsterDatas.count; 

foreach(var data in monsterDatas)
{
    Monster monsterObj = GameObject.Instantiate(m_monsterPrefab) as Monster;
    /// ...
    /// ...
    
    monsters.Add(monsterObj);
}
```

