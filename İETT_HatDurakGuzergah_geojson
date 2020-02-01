#HatDurakGuzergah #https://data.ibb.gov.tr/dataset/sefer-gerceklesme-web-servisi
import zeep  # zeeo kütüphanesi ile SOAP servislerine erişim sağlanır.
import json  # veriyi anlayabilmek için json kütüphanesini kullandık.
import geopandas as gpd 
import shapely.wkt
import requests
wsdl = 'https://api.ibb.gov.tr/iett/UlasimAnaVeri/HatDurakGuzergah.asmx?wsdl'
client = zeep.Client(wsdl=wsdl)
data = client.service.GetDurak_json(DurakKodu="") #İETT API dökümanında belirtilen servis (https://data.ibb.gov.tr/dataset/iett-hat-durak-guzergah-web-servisi/resource/6efd7520-0fbf-421b-975a-a73cb9137ef2)
data = json.loads(data)
# yeni bir geojson dosyası oluşturuyoruz.
new_geojson = {
    'type': 'FeatureCollection',
    'features': []
}
#string olan koordinatları parse edip tanımlayarak shapely yardımıyla istenen geometrik standarda dönüştürüyoruz.
for i in data:
    point = i.get("KOORDINAT")
    p = shapely.wkt.loads(point)
    geojson_data = gpd.GeoSeries([p]).__geo_interface__   #geopandas geoseries kütüphanesi kullanıyoruz.ve boş bir geojson oluşturuyoruz.
    new_geojson["features"].append({'type': 'Feature',   # üretilen boş geojson içerisine apide yer alan propertiesleri iliştiriyoruzx.
                                    'properties': {
                                        'SDURAKKODU': i.get("SDURAKKODU"),
                                        'SDURAKADI': i.get("SDURAKADI"),
                                        'ILCEADI' :i.get("ILCEADI"),
                                        'AKILLI': i.get("AKILLI"),
                                        'FIZIKI': i.get("FIZIKI"),
                                        'DURAK_TIPI': i.get("DURAK_TIPI"),
                                        'SYON': i.get("SYON"),
                                    },
                                    'geometry': geojson_data.get("features")[0].get('geometry')
                                    })
for j in new_geojson.get('features'): oluşan yeni geojson ın her bir feature ı tekrar for döngüsü ile oluşturuluyor.
    print(j)
    
with open('data.geojson', 'w') as json_file:  #dosyaya yazmak isterseniz bu kodu kullanabilirsiniz.
 json.dump(new_geojson, json_file)
