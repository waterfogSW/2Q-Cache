# 2Q

## LRU
LRU는 Least Recently Used의 약자로 페이지 폴트가 발생하게 되면 가장 오래전에 접근했던 페이지를 퇴출시켜 공간을 확보합니다.

버퍼에 존재하는 페이지에 접근하는 경우 리스트의 head로 페이지를 이동시킵니다. 이러한 방식으로 최근에 접근한 페이지 일수록 head에 가깝게, 오래전에 접근한 페이지일수록 tail에 가까이 위치하게 됩니다. 새로운 페이지를 위한 공간이 필요할 경우 tail에 존재하는 페이지를 퇴출시킵니다. 

LRU는 주로 이중 연결리스트와 해시테이블을 통해 구현합니다. 구현이 간단하고, 알고리즘의 효율성 때문에 80년대 초반까지 대부분의 시스템에서 차용되었던 알고리즘입니다. 하지만, 이러한 장점들에도 불구하고 특정 상황에서 최악의 성능을 보입니다.

![img](https://user-images.githubusercontent.com/4745789/100534745-43ae8400-3238-11eb-8855-752a6ef2f3c6.png)

## Sub-Optimality during DB scans

데이터베이스 테이블이 LRU 캐시보다 큰 경우, 테이블을 스캔할 때 DB 프로세스는 전체 LRU 캐시를 삭제하고 스캔한 테이블의 페이지들로 채우게 됩니다. 이러한 페이지들이 다시 참조되지 않는다면 데이터 베이스의 성능을 크게 저하시킬 수 있습니다.

## 2Q
LRU의 이러한 단점을 보완하기 위해 2Q는 추가적인 대기열을 통해 실제로 Hot한 페이지가 LRU캐시에 위치할 수 있도록 합니다. 즉, 2Q는 접근 빈도 뿐만 아니라, 페이지의 Hot여부도 판단합니다.

## Simplified 2Q

![img](https://user-images.githubusercontent.com/4745789/100536835-41a0f100-3249-11eb-920b-0bcaff905906.png)

## 2Q Full Version

![img](https://user-images.githubusercontent.com/4745789/100538168-0bb53a00-3254-11eb-8f69-ddcaf8d33a84.png)

## References

- [LRU - Wikipedia](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU))
- [The Saga of the ARC Algorithm and Patent](http://www.varlena.com/GeneralBits/96.php)
- [2Q: A low overhead high-performance buffer management replacement algorithm](https://www.semanticscholar.org/paper/2Q%3A-A-Low-Overhead-High-Performance-Buffer-Johnson-Shasha/5fa357b43c8351a5d8e7124429e538ad7d687abc)