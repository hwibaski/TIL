# HashMap

## 개요
- 대표적인 Java Collection Library 중 하나
- Map 인터페이스 상속
- `키 :  값` 형태를 가지고 있다
- 값은 중복 저장될 수 있지만 키는 중복 저장될 수 없다.
- 일반적으로 put과 get의 시간 복작도는 logN이다.


## 대표적 메서드 동작방식
 
### 1. put
1. HashMap 내부에 자료를 관리하는 테이블 또는 버킷(이하 테이블)이 초기화 되어 있지 않으면 초기화 한다.
   1. 이 때 기본적으로는 8개의 배열로 선언된다.
2. 키의 hash를 table의 길이로 모듈러 연산하여 데이터가 배치될 index를 구한다.
3. 값을 할당하기 전 해당  table의 해당 index가 null인지 아닌지 확인한다.
4. null이면 
   1. 그 인덱스에 새로운 node(HashMap 내부적으로 사용되고 있는 class)를 할당한다
5. null이 아닐 경우
   1. 기존에 있었던 node와 새로 추가될 node의 key와 hash를 비교하여 결과값이 같다면 새로 추가될 node로 교체한다(값 교체 발생)
   2. 기존의 노드와 키는 다른데 hash가 같다 또는 hash가 다른데 키는 같을 때(해시 충돌이 발생한 경우 동작방식)
      1. 해당 node가 TreeNode 라면 putTreeVal()을 호출하여 트리에 Node를 추가한다. 
      2. 해당 node의 next값이 존재하는지 LinkedList를 순회한다.
         1. 순회 도중 키와 hash가 같은 node가 있다면 node를 교체한다
         2. 순회 도중 키와 hash가 같은 게 없으면 LinkedList의 tail에 node를 추가한다
         3. LinkedList의 node개수가 8개 이상(TREEIFY_THRESHOLD 상수)이면 LinkedList를 Tree 자료 구조로 변경한다.

> 해시테이블 구조에 따른 해시충돌 문제는 Separate Chaining (다른 방식으로는 Open Addressing 방식이 있음)으로 관리하고 있음.
> LinkedList로 관리하던 자료구조를 Tree자료 구조로 변환한다. 

#### 트리노드를 사용하는 이유
- 해시테이블 특성상 충돌이 많이 일어나게될거고 Tree 자료구조 O(logN)의 성능이 되기 때문에 트리를 사용한다.

#### 왜 하필 8개일까?
- 8이라는 값은 실험값일 확률이 높다. Tree 자료구조는 일반적으로 LinkedList보다 조회(Traverse)에서 성능은 좋은데 관리되고 있는 Node가 8개 이하일 때는 LinkedList가 더 빠른 성능을 제공하지 않았을까라고 추측한다.

#### table의 resize() 또는 초기화는 어떻게 하는건가?
1. put 메서드 호출 시 table이 null이라면 HashMap 내부에 선언되어 있는 상수값 16으로 table의 길이를 설정한다.
2. table의 75% (HashMap 내부에 선언되어 있는 LoadFactor(0.75))가 차게 되면은 테이블의 길이를 두 배로 늘려서 리사이징한다. 테이블의 길이만 늘릴 뿐 아니라 요소들을 다시 배치한다.
   1. why? 해시테이블이 테이블의 길이와 모듈러 연산을 하여 요소를 배치하므로 테이블의 길이가 변경되었다면 그에 따른 재배치 필요
   2. 리사이즈는 HashMap 내부에서 꽤 무거운 작업이다. 그러므로 HashMap에 저장될 요소의 개수를 미리 알고 있다면 적절한 값을 이용하여 resize가 최소화되도록 HashMap을 초기화 시켜주는 것이 좋다.

#### LinkedList가 8개 이상부터 무조건 Tree로 변환되는 것은 아니다
table의 전체 길이가 64개보다 작으면 tree로 변경되지 않는다.


#### put method의 내부구현 putVal()
```java
class HashMap<K, V> extends AbstractMap<K, V> implements Map<K, V>, Cloneable, Serializable {
    // ... 중략
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
        Node<K, V>[] tab; // table 선언
        Node<K, V> p; // 추가할 노드 선언
        int n, i;
        
        // (1) tab에 instance 변수 table 할당
        // - tab이 null 이거나 tab 배열의 길이가 0이면
        if ((tab = table) == null || (n = tab.length) == 0)
            // tab을 resize()하고, 그 길이를 n에 할당
            n = (tab = resize()).length;
        
        // i = (n - 1) & hash -> (tab의 길이 - 1) & (hash) -> 해싱 알고리즘 이용해서 인덱스 얻기
        // tab[i]가 null 이면
        if ((p = tab[i = (n - 1) & hash]) == null)
            // tab의 i번째에 새로운 노드를 할당
            tab[i] = newNode(hash, key, value, null);
        // tab[i]가 null이 아니면 즉, 이미 해당 인덱스에 노드가 있으면
        else {
            Node<K, V> e; // 새로운 노드 선언
            K k; // key를 담고 있을 변수 추가
            
            // p의 hash가 putVal의 매개변수로 받은 hash와 같고
            // p.key와 key가 같거나 key가 null이 아니고 key가 k와 같거나 할 때
            // 이 조건이 무엇을 의미하는지 조사 필요 -> hash가 같고 key가 같으면?
            // e = p
            if (p.hash == hash && ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            
            // p가 TreeNode의 인스턴스 일 때
            else if (p instanceof TreeNode)
                // 트리노드의 val로 추가
                e = ((TreeNode<K, V>) p).putTreeVal(this, tab, hash, key, value);
            else {
                // 트리노드가 아닐 때(linkedList 일 때)
                for (int binCount = 0; ; ++binCount) {
                    
                    // p.next가 null일 때 -> 해당 linkedList의 tail일 때
                    if ((e = p.next) == null) {
                        // 새로운 노드를 가장 마지막에 할당
                        p.next = newNode(hash, key, value, null);
                        // binCount가 TREEFFY_THRESHOLD(8) - 1 보다 크거나 같을 때
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            // 링크드 리스트를 트리화 하는건가?
                            treeifyBin(tab, hash);
                        break;
                    }
                    
                    //
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
           
            // e가 null이 아니면 (hash와 key가 같은 값이 있으면 해당 node의 값을 변경)
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
    
}
```
### 2. get
1. table null validation, table 배열의 길이 확인, table[key의 해시값의 모듈려 연산결과]의 null check
2. table[i]의 요소의 key, hash와 조회하고자 하는 key, hash 값이 일치한다면 해당 node 바로 return
3. 그렇지 않다면 요소의 next null check
4. 요소가 트리노드라면 getTreeNode() 메서드 호출로 요소 찾기
5. 트리노드가 아니라면 linkedList 순회하여 요소 찾기

#### get 메서드의 내부 구현 getNode()
```java
class HashMap {
    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        // table의 길이가 0이 아니고, 해당 index가 0이 아닐 경우에
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            
            // 해당 index의 key와 hash가 모두 같은 경우 첫번째 노드 return
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            
            // 해당 인덱스의 첫번째 요소의 next가 존재할 경우 (여러 개의 요소가 존재할 경우)
            if ((e = first.next) != null) {
                // 만약에 first 노드가 treeNode 일 경우
                if (first instanceof TreeNode)
                    // 트리 노드에서 해당 노드 찾아서 리턴
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                // 트리노드가 아닐 경우 linkedList에서 찾아서 리턴
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
}
```

### 3. remove
1. table validation
2. 지울 노드 검색
3. 지울 노드를 찾았다면
   1. 트리 노드일 경우
      1. removeTreeNode() 호출하여 트리 노드의 삭제 -> 삭제라기보다 node의 끼리의 참조를 끊는다.
   2. 2에서 검색한 노드와 지우고자 하는 node와 일치하면 삭제

> remove 되어서 table 내의 요소의 개수가 줄어들었다고 해서 그에 맞게 다시 resize 하지는 않는다.
   

```java
class HashMap {
    final Node<K,V> removeNode(int hash, Object key, Object value,
        boolean matchValue, boolean movable) {
        Node<K,V>[] tab; Node<K,V> p; int n, index;
        
    // table validation, 해당 index의 node 찾기
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (p = tab[index = (n - 1) & hash]) != null) {
            Node<K,V> node = null, e; K k; V v;
            
            // 지울 노드 찾기
            // hash 와 key 확인 후 일치하면 node 변수에 할당
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                node = p;
            
            // 지울 노드의 next가 존재할 경우 (여러개의 요소가 존재할 경우)
            else if ((e = p.next) != null) {
                // TreeNode일 경우
                if (p instanceof TreeNode)
                    node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
                else {
                    // linkeList 일 경우, 충돌이 없어서 요소가 하나인 경우 모두 아래의 연산에서 해결
                    do {
                        if (e.hash == hash &&
                            ((k = e.key) == key ||
                                (key != null && key.equals(k)))) {
                            node = e;
                            break;
                        }
                        p = e;
                    } while ((e = e.next) != null);
                }
            }
            
            // node가 null이 아니고, node의 value가 value와 같거나, value가 null이 아니거나, v 
            if (node != null && (!matchValue || (v = node.value) == value ||
                (value != null && value.equals(v)))) {
                
                // 트리 노드 일경우 removeTreeNode()
                if (node instanceof TreeNode)
                    ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
                
                // node가 p와 같다면 (linkedList 아닐 경우)
                else if (node == p)
                    // tab[index] = null; 이 된다.
                    tab[index] = node.next;
                // tree node도 아니고, node가 p와 같지 않다는 건, 해당 index에 여러개의 Node가 있다는 뜻
                else
                    // 링크를 변경함, GC가 지울 Node 수거해감
                    p.next = node.next;
                ++modCount;
                --size;
                afterNodeRemoval(node);
                return node;
            }
        }
        return null;
    }
}
```

## HashMap 이용 시 주의사항 (어떻게 하면 HashMap을 가장 비효율적으로 사용할 수 있을까?)
- Hash 충돌을 많이 발생시킬수록 비효율적이다.
   - why? HashMap은 table의 요소의 개수가 테이블의 일정 이상(75%)을 넘어가면 리사이즈를 한다. 리사이즈하면 리소스는 더 들어가지만 동작 시 더 효율적이게 된다.
   - 그렇다면 요소는 추가하지만 resize를 일으키지 않게 하면 비효율적으로 사용하는 것이다.
   - 예를들어 table의 길이는 초기값인 16으로 그대로 유지한 상태로 충돌 관리를 위한 tree 자료 구조에 계속적으로 새로운 node가 쌓이게 되면 table은 리사이즈 되지 않고 트리는 계속적으로 비대해진다.
   - 그러므로 hashCode와 equals 메서드를 잘 정의해야 한다.

### reference
https://johngrib.github.io/wiki/java/hashmap/
https://d2.naver.com/helloworld/831311
https://velog.io/@alsgus92/ConcurrentHashMap%EC%9D%98-Thread-safe-%EC%9B%90%EB%A6%AC

