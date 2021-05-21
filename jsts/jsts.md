# JSTS
- 자바스크립트 라이브러리
- buffer, intersect, difference, union, contain 등 제공
- 개발중인 소스와 화면은 복붙 및 캡쳐할 수 없어서 대충 써봄

### buffer
- jsts parser.read() 사용하여 buffer 그릴 feature의 geometry를 읽음
- jsts내 buffer() 함수를 사용
  - 괄호안에 숫자(미터 단위) 들어감
  - geometry가 리턴됨
- return된 geometry를 parser.write() 사용하여 다시 변환
- 해당 feature에 setGeometry 해줌

```javascript
  // openlayers feature를 사용함  
  var feature = new ol.Feature({ geometry : ... , ...});

  var parser = new jsts.io.OL3Parser();
  var jstsGeo = parser.read(feature.getGeometry());
  var bufferd = jstsGeo.buffer(1000);
  feature.setGeometry(parser.write(buffered));
```

### overlaps, contains
- 둘다 boolean return 함
- 포함되었는지 여부 검사할 때 씀
  - ex)
    - 버퍼(1000m) 내에 존재하는 grid인지
    - 선택한 시, 군, 구 영역 내에 feature가 존재하는지
    - ...

```javascript
  var parser = new jsts.io.OL3Parser(); // jsts parser 선언

  var bufferFeature = ...; // 버퍼 feature 1개
  var gridFeatures = [..., ..., ...]; // grid feature 여러개

  var bufferGeo = parser.read(bufferFeature.getGeometry());

  gridFeatures.forEach(function(e) {
    var gridGeo = parser.read(e.getGeometry());  
    
    var overlaps = bufferGeo.overlaps(gridGeo);
    var contains = bufferGeo.contains(gridGeo);

    // 검사
    if (overlaps || contains) {
    	// buffer 내에 grid 존재함
    	// 여기서 넣던 빼던 색깔을 바꾸던 하면됨    	
    }
  });

```

### union
- feature 합치기
- 그냥 진짜 말 그대로 합침
- 나는 뭔가 많은 line들을 합쳐야되서 써봤음
  - 합친거에 합치고 합치고 합치고  

```javascript
  var sampleGeo1 = parser.read(feature1.getGeometry());
  var sampleGeo2 = parser.read(feature2.getGeometry());

  // var unionGeo = jsts.operation.overlay.OverlayOp.union(sampleGeo1, sampleGeo2);
  var unionGeo = sampleGeo1.union(sampleGeo2);
  
  parser.write(unionGeo);

  ...

```

### intersection
- 중첩
- 교집합 return

```javascript
  var sampleGeo1 = parser.read(feature1.getGeometry());
  var sampleGeo2 = parser.read(feature2.getGeometry());
  
  var interGeo = sampleGeo1.intersection(sampleGeo2);

  var feature = new ol.Feature({
    geometry : parser.write(interGeo)
  });

  ...
```



### jsts 참고
- https://bjornharrtell.github.io/jsts
- https://openlayers.org/en/latest/examples/jsts.html



