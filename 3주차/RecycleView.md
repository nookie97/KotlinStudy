# RecycleView
RecyclerView란 ? 데이터 집합들을 각각의 개별 아이템 단위로 구성하여 화면에 출력해주는 뷰 그룹이며, 수 많은 데이터를 스크롤 가능한 리스트 형태로 표시해주는 위젯

## RecycleView와 ListView의 차이
둘다 동일한 형식의 리스트들을 구현할 때 사용하지만 ListView와 RecyclerView에는 비슷한거 같지만 세밀한 차이가 존재함

![스크린샷 2021-10-26 오전 10 50 22](https://user-images.githubusercontent.com/66652964/138795079-1bfe2a80-1a4b-4e40-93a6-55f9ae3b4f38.png)

이때 ListView보다 RecyclerView를 사용하는 가장 큰 이유는 재사용성 이 훨씬 좋기 때문입니다.

ListView는 ViewHolder 패턴을 사용하시 않고 getView()를 이용하여 View에 접근하는데 이처럼 getView()로 동작하게 되면 리스크 갯수만큼 getView()가 호출 되므로 매우 비효율적이지만 RecyclerView는 ViewHolder를 통해 만든 객체를 재사용하기 때문에 ListView보다 RecyclerView가 재 사용성 부분에서 더 효율적입니다.


## RecyclerView 주요 클래스 

### View Holder 

각각의 뷰를 보관하는 Holder 객체

Item 뷰들을 재활용하기 위해 각 요소를 저장해두고 사용하는데 즉, 아이템 생성시 뷰 바인딩은 한 번만하며, 그 바인딩 된 객체를 가져다 사용하여 성능 부분에서 효율적임

### LayoutManager

 * 아이템의 배치를 담당합니다. 

 * LinearLayoutManager - 가로 / 세로, GridLayoutManager - 그리드 형식, StaggeredGridLayoutManager - 지그재그형의 그리드 형식

### Adapter

 * ListView와 동일한 Adapter의 개념으로 데이터와 아이템에 관한 View를 생성하는 기능을 담당

### Click Detection 

 * Click Listener가 ListView 처럼 내장되어 있지 않으므로, onClickListner를 통해 직접 구현해주어야 함
