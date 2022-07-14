# JavaScript: Google Map API

> Thu Jul 14, 2022

---

[toc]

구글 맵 API 를 활용해봅시다. Google 지도를 사용하기 위해선 Google 지도 API 키를 발급받아 내 사이트에 적용해야 합니다

우선 [Google Maps Platform](https://console.cloud.google.com/google/maps-apis/overview?cloudshell=false&project=original-voyage-356300) 에서 API Key 를 받은후 개발을 위해 잠시 키 제한을 없음으로 설정해놓습니다.



### googleMap.html

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	#googleMapView{
		height:700px;
		border:1px solid #ddd;
	}
</style>
<!-- google 맵 라이브러리를 연결 -->
<script async defer src="https://maps.googleapis.com/maps/api/js?key=[YOURAPI]&callback=initMap"></script>
</head>
<body>
<div id="googleMapView"></div>
<script>
	var latitude = 37.5729503;
	var longitude = 126.9793578;

	var lat = [37.550687, 37.5393413, 37.537437];
	var log = [126.972688, 126.9613324, 126.993938];
	function initMap(){
		// 위도, 경도를 이용한 객체생성
		var myCenter = new google.maps.LatLng(latitude, longitude);
		
		// 표시할 지도 옵션를 JSON 형식으로 만들어 라이브러리 연결시 전달
		var mapProperty = {
				center : myCenter, // 가운데 위치 설정
				zoom : 14, // 0~21 사이의 값을 사용한다. 숫자가 클수록 확대.
				mapTypeId : google.maps.MapTypeId.ROADMAP // 지도종류(ROADMAP, HYBRID, STEELITE, TERRAIN) 
		};
		// 									지도를 표시할 곳 					 	맵옵션
		var map = new google.maps.Map(document.getElementById("googleMapView"), mapProperty);
		
		// 마커표시
		var marker = new google.maps.Marker({
			position: myCenter, // 마커를 표시할 위치
			map: map,// 마커를 표시할 지도
			title: "Seoul City Hall", // 마커에 마우스오버시 표시할 내용
			// icon: "gmap_pin.png"
		});
		
		// 정보대화상자
		var infoMsg = "위도:"+latitude+"<br/>경도="+longitude;
		infoMsg += "<br/><a href='https://www.seoul.go.kr'><img src='../../img/03.png' width='100' height='100'/></a>"
		
		var info = new google.maps.InfoWindow({
			content: infoMsg
		});
		
		// 이벤트 등록				  이벤트발생대상, 이벤트종류, 대화상자  
		google.maps.event.addListener(marker, 'click', function(){info.open(map, marker);});
		
		// 마커 여러곳 표시하기
		for(var i=0; i<lat.length; i++){
			var mk = new google.maps.Marker({
				position:new google.maps.LatLng(lat[i], log[i]),
				map: map,
				title: "위도="+lat[i]+", 경도="+log[i]
			});
			mk.setMap(map);
		}
		// 반경표시
		var myCity = new google.maps.Circle({
			center: myCenter, // 반경의 중심점
			radius: 500, // 반지름 m
			strokeColor: '#f00', // 선의 색상
			strokeWidth: 4, // 선의 두께 px 
			strokeOpacity: 0.5, //선의 투명도 0~1
			fillColor: '#0f0', // 면의 색상
			fillOpacity: 0.5 // 면의 투명도
		});
		myCity.setMap(map);
	}
</script>
</body>
</html>
```



### googleMapGeocode.html

> 지오코딩은 주소(예: '1600 Amphitheatre Parkway, Mountain View, CA')를 지리적 좌표(예: 위도 37.423021, 경도 122.083739)로 변환하는 프로세스입니다. 이 지리적 좌표를 사용하여 위치 아이콘을 표시하거나 지도를 배치할 수 있습니다.

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	#gMap{
		height: 700px; border:1px solid gray;
	}
	#geocodeSearch{
		position:absolute;
		left:40%;
		top:50px;
		z-index:100;
	}
</style>
<!-- google 맵 라이브러리를 연결 -->
<script async defer src="https://maps.googleapis.com/maps/api/js?key=[YOURAPI]&callback=initMap"></script>
</head>
<body>
<div>
	<div id="geocodeSearch">
		<input type="text" id="address" value="Fisherman's Wharf"/>
		<input type="button" value="GeoCode" id="geoSubmit" onClick="searchMap()"/>
	</div>
	<div id="gMap"></div>
</div>
<script>
	var latitude;
	var longitude;
	var map;
	var addr;
	var home;
	var img;
	var geoCode;
	
	// 정보를 셋팅하는 함수
	function setMapData(){
		latitude = 37.81180;
		longitude = -122.27126;
		
		addr = ["Fisherman's Wharf, San Francisco", "The Palace Of Fine Arts, San Francisco", "Oceanview Diner, Berkeley", "Salito's Crab House & Prime Rib"];
		home = ["https://www.fishermanswharf.org/", "https://palaceoffinearts.com/","https://breakfastberkeley.com/", "https://www.salitoscrabhouse.com/"];
		img = ['sf01.jpeg', 'sf02.jpeg', 'sf03.jpeg', 'sf04.jpeg'];	
	}
	// 구글맵이 호출하는 함수
	function initMap(){
		setMapData();
		
		var myCenter = new google.maps.LatLng(latitude, longitude);
		var option = {
				center: myCenter,
				zoom: 12,
				mapTypeId: google.maps.MapTypeId.ROADMAP
		}
		map = new google.maps.Map(document.getElementById("gMap"), option);
		
		// 마커표시
		var marker = new google.maps.Marker({
			position: myCenter, // 마커를 표시할 위치
			map: map,// 마커를 표시할 지도
			title: "My Home", // 마커에 마우스오버시 표시할 내용
		});
		
		// 지명을 이용한 지도 표시를 위해서는 Geocode 객체가 필요하다 
		geoCode = new google.maps.Geocoder();
		
		// 배열의 지명을 이용하여 마커 표시하기
		for(var idx=0; idx<addr.length; idx++){
			setMapPosition(addr[idx], home[idx], img[idx]);
		}
	}
	// 위치 이동하는 함수
	function setMapPosition(mapAddr, mapHome, mapImg){
		
		// 				지도에서 찾을 위치		 콜백함수( 찾은 정보, 성공여부)
		geoCode.geocode({'address':mapAddr}, function(results, status){ // status='OK' 성공시
				if(status=="OK"){ // 위치를 찾았을때
					console.log(results);
					// 지도의 중앙위치를 변경				  위도, 경도 객체
					map.setCenter(results[0].geometry.location);
					
					var marker = new google.maps.Marker({
						map:map,
						icon: 'gmap_pin.png',
						title: results[0]['formatted_address'], // 주소
						position: results[0].geometry.location // 위도 경도 객체
					});
					
					var la = results[0].geometry.location.lat(); // results[0]['geometry']['location']['lat']()
					var lo = results[0]['geometry']['location']['lng']();
					
					var popMsg = "Latitude : "+la;
						popMsg += "<br/>Longitude : "+lo;
						popMsg += "<br/>Address : " + results[0].formatted_address;
						popMsg += "<br/><a href='"+mapHome+"'><img src='../../img/"+mapImg+"'/></a>";
					
					var info = new google.maps.InfoWindow({content:popMsg});
					
					// 마카에 마우스 오버시 info 보여주기
					google.maps.event.addListener(marker, 'mouseover', function(){
						info.open(map, marker);
					});
					// info 마우스 아웃시 info 숨기기
					google.maps.event.addListener(info, 'mouseout', function(){
						info.close(map, marker);
					});
				}else{ // 위치를 못찾았을때
					alert("no result found");
				} // if
			} // function()
		); // geocode(); 
		
	}
	// 지역명을 이용한 지도 검색
	function searchMap(){
		var addr = document.getElementById("address").value
		if(addr == ""){
			alert("Search Google Maps.");
		}else{
			setMapPosition(addr, "https://www.moranapts.com/#", 'sf05.jpeg');
		}
		
	}
	
</script>
</body>
</html>
```

