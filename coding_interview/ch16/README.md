# 16장 트라이
트라이는 검색 트리의 일종으로 일반적으로 키가 문자열인, 동적 배열 또는 연관 배열을 저장하는 데 사용되지 정렬된 트리 자료구조다.
트라이는 실무에 매우 유용하게 쓰이는 자료구조로서, 특히 자연어 처리 분야에서 문자열 탐색을 위한 자료구조로 널리 쓰인다.

트라이는 각각의 문자 단위로 색인을 구축한 것과 유사하며 자연어 처리 분야에서는 형태소 분석기에서 분석 패턴을 트라이로 만들어두고 자연어 문장에 대해 패턴을 찾아 처리하는 등으로 활용하고 있다.

## 56 트라이 구현

`트라이의 insert, search, startswith 메소드를 구현하라.`

Trie trie = new Trie();

trie.insert("apple");<br>
trie.search("apple");   // returns true<br>
trie.search("app");   // returns false<br>
trie.startsWith("apple");   // returns true<br>
trie.insert("app");<br>
trie.search("app");   // returns true<br>

## 풀이1 딕셔너리를 이용해 간결한 트라이 구현

```
class TrieNode:
    def __init__(self):
        self.word = False
        self.children = collections.defaultdict(TrieNode) 
```
- 트라이를 저장할 노드는 다음과 같이 별도의 클래스로 선언


    class Trie:
      def __init__(self):
        self.root = TrieNode()

    # 단어 삽입
    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            node = node.children[char]
        node.word = True

- 삽입 시 문자 단위의 다진 트리형태가 된다.

![1.png](data:image/png;base64,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)


    # 단어 존재 여부 판별
    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]

        return node.word

- 문자열에서 문자를 하나씩 for 반복문으로 순회하면서 자식 노드로 계속 타고 내려가 마지막에 node.word 여부를 리턴한다.


    # 문자열로 시작 단어 존재 여부 판별
    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]

        return True

- search()와 거의 동일하나 차이점은 node.word를 확인하지 않고, 자식 노드가 존재하는지 여부만 판별한다는 점이다.


```python
import collections


# 트라이의 노드
class TrieNode:
    def __init__(self):
        self.word = False
        self.children = collections.defaultdict(TrieNode) 

class Trie:
    def __init__(self):
        self.root = TrieNode()

    # 단어 삽입4
    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            node = node.children[char]
        node.word = True

    # 단어 존재 여부 판별
    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]

        return node.word

    # 문자열로 시작 단어 존재 여부 판별
    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]

        return True

if __name__ == '__main__':
    t = Trie()
    t.insert('apple')
    print(t.search('apple'))
    print(t.search('app'))
    print(t.startsWith('app'))
    t.insert('app')
    print(t.search('app'))
```

    True
    False
    True
    True
    

## 57 팰린드롬 페어

`단어 리스트에서 words[i] + words[j]가 팰린드롬이 되는 모든 인덱스 조함 (i, j)를구하라.`

예제 1<br>
- 입력<br>
["abcd","dcba","lls","s","sssll"]<br>

- 출력<br>
[[0,1],[1,0],[3,2],[2,4]]

예제 2<br>

- 입력<br>
["bat","tab","cat"]
- 출력<br>
[[0.,1],[1,0]]

## 풀이1 팰린드롬을 브루트 포스로 계산 

- 브루트 포스 <br>
고지식한 패턴검색으로 본문의 처음부터 끝까지 차례대로 순회하면서 패턴 내의 문자들을 일일이 비교하는 방식



```python
from typing import List


class Solution:
    def palindromePairs(self, words: List[str]) -> List[List[int]]:
        def is_palindrome(word):
            return word == word[::-1]

        output = []
        for i, word1 in enumerate(words):
            for j, word2 in enumerate(words):
                if i == j:             
                    continue
                if is_palindrome(word1 + word2):
                    output.append([i, j])
                    print("i:",i)
                    print("j:",j)
        return output

if __name__ == '__main__':                                                                       
    s = Solution()
    print(s.palindromePairs(["abcd","dcba","lls","s","sssll"]))
```

    i: 0
    j: 1
    i: 1
    j: 0
    i: 2
    j: 4
    i: 3
    j: 2
    [[0, 1], [1, 0], [2, 4], [3, 2]]
    

## 풀이2 트라이 구현


```
# 트라이 저장할 노드
class TrieNode:
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.word_id = -1
        self.palindrome_word_ids = []
```

- palindrome_word_ids느 리스트이며 복수형이다.

```
class Trie:
    def __init__(self):
        self.root = TrieNode()

    @staticmethod # 정적메소드로 인스턴스메소드와 달리 self라는 인자를 가지고있지 않다
    def is_palindrome(word: str) -> bool:
        return word[::] == word[::-1]
```

- 파이썬 자료형의 인스턴스 메서드와 정적 메서드 차이<br>
a = {1, 2, 3, 4}<br>
a.update({5})    # 인스턴스 메서드<br>
a<br>
{1, 2, 3, 4, 5}<br>
set.union({1, 2, 3, 4}, {5})    # 정적(클래스) 메서드<br>
{1, 2, 3, 4, 5}

```
    # 단어 삽입
    def insert(self, index, word) -> None:
        node = self.root
        for i, char in enumerate(reversed(word)):
            if self.is_palindrome(word[0:len(word) - i]): 
                node.palindrome_word_ids.append(index)
            node = node.children[char]
        node.word_id = index
```

- 단어를 뒤집고 문자단위로 내려가며 트라이 구성 
- 그 후 각각의 단어가 끝나는 지점에 단어인덱스를 word_id로 부여

        # 판별 로직 1 : 끝까지 탐색했을 때 word_id가 있는경우
        if node.word_id >= 0 and node.word_id != index:
            result.append([index, node.word_id])

- 입력값을 순서대로 탐색
- 끝나는 지점의 word_id 값이 -1이 아니라면 현재 인덱스와 해당 word_id는 팰린드롬으로 판단할 수 있다.

        # 판별 로직 2 : 끝까지 탐색했을 때 palindrome_word_ids가 있는 경우 
        for palindrome_word_id in node.palindrome_word_ids:
            result.append([index, palindrome_word_id])

- 트라이 삽입 중 남아 있는 단어가 팰린드롬이라면 미리 팰린드롬 여부를 판단하게 세팅한 것이다.

            # 판별 로직 3 : 탐색 중간에 word_id가 있고 나머지 문자가 팰린드롬인 경우
            if node.word_id >= 0:
                if self.is_palindrome(word):
                    result.append([index, node.word_id])
            if not word[0] in node.children:
                return result
            node = node.children[word[0]]
            word = word[1:]

- 판별 로직은 입력값을 문자 단위로 확인해 나가다가 해당 노드의 word_id가 -1이 아닐 때, 나머지 문자가 팰린드롬이라면 팰린드롬으로 판별하는 경우다.

예 : dcbc + d


마지막으로 앞서 브루트 포스가 타임아웃인 데 반해 트라이 풀이는 시간 내에 잘 실행이 된다


```python
import collections
from typing import List


# 트라이 저장할 노드
class TrieNode:
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.word_id = -1
        self.palindrome_word_ids = []


class Trie:
    def __init__(self):
        self.root = TrieNode()

    @staticmethod # 정적메소드로 인스턴스메소드와 달리 self라는 인자를 가지고있지 않다
    def is_palindrome(word: str) -> bool:
        return word[::] == word[::-1]

    # 단어 삽입
    def insert(self, index, word) -> None:
        node = self.root
        for i, char in enumerate(reversed(word)):
            if self.is_palindrome(word[0:len(word) - i]): 
                node.palindrome_word_ids.append(index)
            node = node.children[char]
        node.word_id = index

    def search(self, index, word) -> List[List[int]]:
        result = []
        node = self.root

        while word:
            # 판별 로직 3 : 탐색 중간에 word_id가 있고 나머지 문자가 팰린드롬인 경우
            if node.word_id >= 0:
                if self.is_palindrome(word):
                    result.append([index, node.word_id])
            if not word[0] in node.children:
                return result
            node = node.children[word[0]]
            word = word[1:]

        # 판별 로직 1 : 끝까지 탐색했을 때 word_id가 있는경우
        if node.word_id >= 0 and node.word_id != index:
            result.append([index, node.word_id])

        # 판별 로직 2 : 끝까지 탐색했을 때 palindrome_word_ids가 있는 경우 
        for palindrome_word_id in node.palindrome_word_ids:
            result.append([index, palindrome_word_id])

        return result


class Solution:
    def palindromePairs(self, words: List[str]) -> List[List[int]]:
        trie = Trie()

        for i, word in enumerate(words):
            trie.insert(i, word)

        results = []
        for i, word in enumerate(words):
            results.extend(trie.search(i, word))

        return results


if __name__ == '__main__':
    s = Solution()
    print(s.palindromePairs(["bat","tab","cat"]))
```

    [[0, 1], [1, 0]]
    
