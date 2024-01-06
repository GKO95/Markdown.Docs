# 디스크
**[디스크](https://en.wikipedia.org/wiki/Disk_storage)**(disk)는 회전하는 표면에 전기적, 자기적, 광학적, 또는 기계적으로 데이터를 기록하고 읽을 수 있는 원판을 가리킨다. 이러한 저장 매커니즘을 구현한 장치를 **디스크 드라이브**(disk drive)라고 부른다. 대표적으로 디스크 저장 장치로 [HDD](https://en.wikipedia.org/wiki/Hard_disk_drive), [CD](https://en.wikipedia.org/wiki/Compact_disc) 및 [DVD](https://en.wikipedia.org/wiki/DVD), [플로피 디스크](https://en.wikipedia.org/wiki/Floppy_disk) 등이 존재한다. 본 문서는 [자기식 저장](https://en.wikipedia.org/wiki/Magnetic_storage) 매커니즘을 활용한 HDD(일명 하드 드라이브)를 위주로 설명한다.

## 디스크 구조
다음은 디스크의 구조를 설명하기 앞서 이해를 돕기 위한 그림을 보여준다.

![디스크 구조](https://upload.wikimedia.org/wikipedia/commons/a/ae/Disk-structure2.svg)

<ol type="A"><li><a href="https://en.wikipedia.org/wiki/Track_(disk_drive)">트랙</a>: 자기적으로 정보가 기록되거나 읽어올 수 있는 디스크의 원형 경로</li><li><a href="https://en.wikipedia.org/wiki/Circular_sector">부채꼴</a>: 기하학상 원에서 두 개의 반지름과 하나의 호로 둘러싸인 영역</li><li><a href="#섹터">섹터</a>: 디스크에서 가장 작은 저장 단위</li><li>클러스터</li></ol>

### 섹터
**[섹터](https://en.wikipedia.org/wiki/Disk_sector)**(sector)는 트랙의 일부이며, 디스크에서 가장 작은 저장 단위이다. 대부분 디스크의 섹터들은 담을 수 있는 데이터 용량이 고정되었고, HDD의 경우 512 [바이트](https://en.wikipedia.org/wiki/Byte)이다. 그리고 파일은 실제 크기와 무관하게 정수개의 섹터를 사용하도록 계획되어, 만일 파일이 섹터 전체를 차지하지 못하면 나머지는 영값으로 채워진다.
