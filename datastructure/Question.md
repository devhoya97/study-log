# 자료구조
### 배열과 링크드 리스트의 차이점에 대해서 설명해 주세요.
### 스택과 큐에 대해서 설명해 주세요.
### 우선순위 큐에 대해서 설명해 주세요.
### 해시테이블에 대해서 설명해 주세요.
### 그래프와 트리의 차이점에 대해서 설명해 주세요.
### 최소 신장 트리에 대해서 설명해 주세요.
### B-트리에 대해 설명해주세요.
B-트리는 균형잡힌 다진 검색 트리로서, 두 가지 성질을 만족해야 합니다.
첫 째, 노드의 크기가 k라면, 루트를 제외한 모든 노드는 k/2 ~ k 개의 키를 가져야 합니다.
둘 째, 모든 리프 노드는 같은 깊이를 가져야 합니다.
이는 데이터를 검색하기 위해 방문해야 하는 디스크 페이지의 수를 줄이기 위해서입니다.
검색 작업은 이진 검색 트리의 작업과 같이 직관적이지만, 삽입과 삭제 작업 시 형제 노드 또는 부모 노드와 협력한다는 점이 독특합니다.

### 꼬리질문1. B-트리에서 삽입 과정에 대해 설명해주세요.
검색 작업을 통해 삽입할 키가 들어갈 리프 노드를 찾습니다.
데이터를 삽입했을 때 오버플로우가 발생하지 않으면 간단히 삽입할 수 있지만, 오버플로우가 발생하는 경우에는 양 옆 형제 노드에 자리가 있는지 확인합니다.
형제 노드에 자리가 있다면, 검색된 노드의 끝자락 키를 형제 노드에게 넘겨줍니다.
이 때, 형제 노드에 바로 넘기면 검색 트리의 성질이 깨지는 경우, 부모 노드에 있는 키를 형제 노드에게 옮기고, 원래 넘기려던 키는 부모 노드로 옮겨주는 재분배 작업이 일어날 수 있습니다.
더 심각한 경우는 형제 노드에 자리가 없는 경우입니다. 이 때는 어쩔 수 없이 분할 작업이 일어나게 되며,
부모 노드에 새로운 키 값이 추가되므로, 부모 노드까지 재귀적으로 삽입 작업이 발생하여 경우에 따라 분할까지 발생할 수 있습니다.

### 꼬리질문2. B-트리에서 삭제 과정에 대해 설명해주세요.
검색 작업을 통해 삭제할 키를 갖는 노드를 찾습니다. 찾은 노드가 리프노드가 아니라면, 삭제할 키의 직후 키를 갖는 리프 노드를 찾아 둘을 맞바꿔서 리프노드에서 삭제 작업을 시작합니다.
데이터를 삭제했을 때 언더플로우가 발생하지 않으면 간단히 삭제할 수 있지만, 언더플로우가 발생하는 경우에는 양 옆 형제 노드에 여유가 있는지 확인합니다.
형제 노드에 여유가 있다면, 끝자락 키를 형제 노드로부터 받아와 언더플로우를 해결합니다.
형제 노드에 여유가 없다면, 형제 노드와 병합해야 합니다. 이 때 부모 노드에서도 키가 하나 재귀적으로 제거되므로 경우에 따라 언더플로우가 발생할 수 있습니다.

### 꼬리질문3. B-트리와 B+트리의 차이에 대해 설명해주세요.
- 더 공부해보고 보강하기. 아직 잘 모르겠다.
B+트리의 리프 노드는 연결 리스트 형태이므로, 형제노드끼리도 옮겨가며 조회할 수 있습니다.
연결된 리프 노드의 리스트를 따라가면서 범위 쿼리를 할 수 있어서, 범위 검색 성능이 좋습니다.
internal 노드에는 데이터 포인터도 없으며 오로지 키만 저장됩니다.
데이터를 찾기 위한 포인터는 리프 노드에만 있고, 덕분에 internal 노드의 크기를 줄여 메모리 사용이 효율적입니다.

### 꼬리질문4. MySQL의 InnoDB에서 B-트리 구조를 사용하는 이유는 무엇인가요?

### 힙 자료구조에 대해 설명해 주세요.
우선순위 큐에서 주로 사용되는 자료구조로서, 배열을 이용해 구현할 수 있습니다.
최대힙 기준으로 가장 값이 큰 데이터가 배열의 첫 번째 인덱스에 오도록 재귀적인 작업인 스며오르기와 스며내리기를 활용할 수 있습니다.
이게 가능한 이유는 완전이진트리로 데이터를 표현했을 때, 특정 노드의 인덱스만 알고 있다면,
해당 노드 기준으로 자식 노드는 2k+1, 2k+2, 부모 노드는 (k-1)/2 소수점버림 하여 인덱스를 곧바로 알아낼 수 있기 때문입니다.

### 꼬리질문1. 스며오르기에 대해 설명해주세요.
힙에 새로운 원소를 삽입할 때, 맨 끝에 원소를 추가하고, 부모 노드와 비교했을 때, 
최대힙 기준, 부모 노드보다 값이 크다면 두 값을 맞바꾸는 작업을 재귀적으로 수행하는 과정입니다. 
만약 n개의 원소가 존재하는 힙에 데이터를 추가하려면 O(log_2 N)의 시간복잡도가 필요합니다. 

### 힙의 삽입과 삭제는 어떻게 이루어지나요?
힙의 삽입과 삭제는 각각 스며오르기와 스며내리기에 의해 수행됩니다.
스며오르기의 경우, 힙에 새로운 원소를 삽입할 때, 힙의 맨 끝에 원소를 추가하고, 부모 노드의 원소값과 비교하면서
최대힙 기준, 부모 노드보다 값이 크다면 두 값을 맞바꾸는 작업을 재귀적으로 수행합니다.
스며내리기의 경우, 힙의 가장 첫 번째 원소를 삭제할 때, 일단 힙의 맨 끝 원소를 힙의 가장 첫 번째 위치로 복사하고, 자식 노드의 원소값과 비교하면서
두 자식 중 큰 자식과 비교했을 때 자식의 값이 더 크다면, 둘을 맞바꾸는 작업을 재귀적으로 수행합니다. 
만약 n개의 원소가 존재하는 힙에 데이터를 추가하거나 삭제하려면 O(log_2 N)의 시간복잡도가 필요합니다.

### 임의의 배열을 힙으로 만드는 방법을 설명해주세요.
힙의 마지막 노드의 부모를 루트로 하는 서브 트리를 시작으로, 서브 트리에 대해 스며내리기 작업을 수행하고, 루트를 왼쪽으로 이동시키며 이를 반복합니다.
왼쪽 끝에 도달했으면, 윗 레벨의 오른쪽 끝부터 루트를 지정해서 서브 트리에 대해 스며내리기 작업을 수행하며 왼쪽으로 루트를 쭉 이동시킵니다.
이 순서로 작업해야만 특정 노드를 루트로 했을 때, 루트의 양 옆 서브트리가 모두 힙 조건을 만족하기 때문에, 스며내리기의 조건을 만족할 수 있습니다.

### 꼬리질문. 위 과정의 시간 복잡도는 어떻게 되나요?
O(n)입니다.
힙에 n개의 원소가 있다면, 서브 트리를 구성할 수 있는 루트의 갯수는 n보단 작지만 n의 갯수와 비례합니다.
루트마다 스며내리기 작업을 수행해야 하므로, O(nlogn)이라고 생각할 수 있지만, 아래에 있는 서브트리들은 1번의 비교만 필요하고,
그 윗 레벨 노드를 루트로 하는 서브트리들은 2번의 비교, 그 윗 레벨은 3번의 비교가 필요하면서 레벨이 올라갈수록 비교 횟수가 증가하는데,
힙은 완전이진트리이므로 윗 레벨로 갈수록 최대 원소의 수가 1/2배씩 줄어들기 때문에 이들을 모두 고려하면 O(n)이 되는 것으로 알고 있습니다.

### 이진 탐색 트리에 대해 설명해 주세요.
### 포화(Perfect) 이진트리, 완전(Complete) 이진트리, 정(Full) 이진트리의 차이점에 대해 각각 설명해주세요.
### 레드 블랙 트리에 대해 설명해주세요.
### 레드 블랙 트리의 삽입과 삭제 과정에 대해서 말해보세요.
### AVL에 대해 설명해주세요.
### B-Tree와 B+Tree에 대해서 설명해 주세요.

# 정렬
### 선택정렬, 버블정렬, 삽입정렬을 비교해주세요.
모두 기본정렬에 속하므로 시간 복잡도가 O(n^2)이지만, 이미 어느정도 정렬된 배열을 정렬하려 한다면 삽입정렬은 O(n)도 가능합니다.
선택정렬은 배열에서 가장 큰 원소를 맨 뒤로 보내고, 정렬하는 배열의 크기를 하나씩 줄여나가는 과정입니다. 따라서 이미 정렬된 배열이 들어와도 O(n^2)이 필요합니다.
버블정렬은 배열의 맨 앞에서부터 시작해 자신의 오른쪽보다 값이 크면 맞바꿈으로써, 정렬하는 배열의 크기를 하나씩 줄여나갑니다.
한 번의 for문이 돌 때마다 맞바꾼 값이 있는지 확인하는 flag를 둔다면, 거의 정렬된 배열이 들어왔을 때, 시간복잡도를 크게 줄일 수 있습니다.
선택정렬과 버블정렬은 정렬되지 않은 배열을 하나씩 줄여나가는 방법이라면, 삽입정렬은 정렬된 배열을 하나씩 늘려나가는 방법입니다.
0~n-1까지의 배열이 정렬되어 있다면, n번째 원소를 삽입했을 때 n-1번째 원소부터 왼쪽으로 확인해가며 n번째 원소보다 값이 큰 지 확인하고, 큰 값들은
오른쪽으로 한 칸씩 이동시켜 생긴 빈 자리에 n번째 원소를 삽입합니다. 이미 배열이 거의 정렬되어 있다면 거의 모든 for문이 상수시간만에 끝나므로
전체 for문을 돌리는데 O(n)으로 가능하게 됩니다.

### 병합정렬에 대해 설명해주세요.
배열을 둘로 분할하여 전반부와 후반부를 각각 따로 정렬하고, 이를 하나의 정렬된 배열로 합치는 과정을 재귀적으로 반복합니다.
전반부와 후반부가 각각 정렬되어 있다면, 각자 왼쪽에서부터 포인터를 가지고 값을 비교해가며 합침으로써 하나의 정렬된 배열을 만들 수 있습니다.
어떤 경우든 O(nlogn)의 시간복잡도가 소요되며, merge를 위해 배열 크기만큼의 tmp 배열이 따로 필요하다는 단점이 있습니다. 

### 퀵정렬에 대해 설명해주세요.
병합정렬과 마찬가지로 재귀적으로 문제를 해결하지만, 일반적으로 속도가 더 빠릅니다.
기준값을 잡고, 기준값보다 작은 값들을 1구역에 몰고, 기준값보다 큰 값을 2구역에 몬 뒤, 기준 값을 1구역의 바로 뒤에 놓음으로써
한 번의 메서드가 호출될 때마다 기준값의 위치를 확정해나가는 방법입니다. 
기준값이 정렬하려는 배열의 중간값으로 잡힐수록 1구역과 2구역의 크기가 계속 반토막이 되므로 정렬 속도가 빨라지는데,
반대로 기준값이 극단적인 값으로 잡히면, 메서드가 호출될 때 기준 값의 위치만 정해지고 구역이 전혀 나뉘어지지 않기 때문에
계속 최악의 기준값을 선택한다면 O(n^2)의 시간복잡도가 소요될 수 있습니다.

### 힙정렬에 대해 설명해주세요.
배열을 최대 힙으로 만들고, 힙의 루트를 삭제하는 작업을 힙의 크기만큼 반복하면 배열이 오름차순으로 정렬됩니다.
루트를 삭제할 때, 힙의 가장 마지막 원소를 루트에 복사하게 되는데, 이 때 루트 값을 힙의 가장 마지막 원소 자리에 넣어줍니다.
그 후 힙의 새로운 루트가 스며내리기를 완료하면, 가장 큰 원소가 배열의 맨 끝으로 이동한 상태에서 다시 힙 구조가 유지됩니다.
따라서 이 작업을 반복하면 결국엔 오름차순 배열이 완성됩니다.
스며내리기 작업을 n번 반복해야 하는데, 한 번의 스며내리기가 O(logn)의 시간복잡도를 가지므로,
힙 정렬은 O(nlogn)의 시간복잡도를 갖습니다.
힙 정렬은 일반적으로 병합정렬보다 느리지만, 새로운 배열이 필요하지 않고,
모든 원소가 동일한 경우엔 모든 스며내리기가 O(1)로 끝나므로, 총 O(n)의 시간 복잡도가 소요됩니다.

### 셸정렬에 대해 설명해주세요.
배열이 이미 어느정도 정렬되어 있을 때, 삽입정렬의 효율이 좋다는 점에 착안해서, 배열이 어느정도 정렬되도록 미리 작업해두고 삽입정렬을 수행하는 방법입니다.
배열을 어느정도 미리 정렬하기 위해 갭수열을 사용하며, 갭수열의 마지막 원소는 1이 되어 일반적인 삽입정렬이 수행됩니다.
예를 들어 갭수열이 7, 3, 1이라면, A[0, 7, 14 ...], A[1, 8, 15], ...와 같이 7개의 부분수열로 나눠서 삽입정렬을 수행하고,
같은 방법으로 3개의 부분수열로 나눠서 삽입정렬을 수행하고, 마지막으로 크기 1의 일반적인 삽입정렬을 수행합니다.
셸정렬의 시간복잡도는 갭수열을 어떻게 설정하냐에 따라서 각각 달라지지만, 알려진 가장 좋은 상한은 O(n^1.25)입니다.

# String / Long Data
### String Comparison Complexity에서 시간 복잡도가 최악인 경우는? 발생할 확률은?
### String Comparison Complexity를 개선할 수 있는 방법은? (길이비교, 해시)
### substr(), length(), indexOf() 를 직접 구현해보자
### 비교기반 자료구조/탐색알고리즘에 적용하면 생기는 문제점과 해결책 (hash / graph)
### String Literal과 언어에서 String의 구현방법
### Substring Problem의 종류와 원리를 설명하시오
