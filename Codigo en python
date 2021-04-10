# Mapa de ubicación monopatines en CABA con leyenda

#Importar librerias:
import pandas as pd
import requests
import folium
from folium.plugins import MiniMap

#Clave y token generada en https://www.buenosaires.gob.ar/desarrollourbano/transporte/apitransporte:
p_client_id='XXX'
p_client_secret='XXX'

#URL:
api_ecobicis='https://apitransporte.buenosaires.gob.ar/ecobici/gbfs/stationInformation?'

#Conexion a la API:
request_ecobicis=requests.get(url=api_ecobicis,
                    params={'client_id':p_client_id,
                            'client_secret':p_client_secret}
                    )
#Validar status 200:
request_ecobicis.status_code

#Generar DataFrame a partir del json:
request_ecobicis.json()

pd.json_normalize(request_ecobicis.json())
pd.json_normalize(request_ecobicis.json(),meta=['data'])
dfecobicis=pd.json_normalize(request_ecobicis.json()['data']['stations'])

#Analisis de DF:
dfecobicis.head()
dfecobicis.shape
dfecobicis.info()
dfecobicis.isna().sum()
dfecobicis.describe()

#Analisis capacidades por estacion:
pd.DataFrame(
            dfecobicis.value_counts('capacity')
            ).reset_index().rename(
                                  {0:'Cantidad'},axis=1
                                  ).sort_values('capacity')
                                  
#Creación del mapa (marker obtenido de https://getbootstrap.com/docs/3.3/components/):
#Lat y long de inicio:
Latitud=-34.628847
Longitud=-58.377551

m_ecobicis=folium.Map(location=[Latitud,Longitud],zoom_start=10,tiles='Stamen Terrain')
for i in range(0,dfecobicis.shape[0]):
    lat=dfecobicis.loc[i,'lat']
    long=dfecobicis.loc[i,'lon']
    popup=dfecobicis.loc[i,'capacity']
    if dfecobicis.loc[i,'capacity']<=20:
        folium.Marker(location=[lat,long],
                      tooltip=popup,
                      icon=folium.Icon(color='orange',icon='road')
                      ).add_to(m_ecobicis)
    else:
        folium.Marker(location=[lat,long],
                      tooltip=popup,
                      icon=folium.Icon(color='blue',icon='road')
                      ).add_to(m_ecobicis)
    folium.CircleMarker(location=[lat,long],
                        radius=5,fill=True,fill_color='blue'
                       ).add_to(m_ecobicis)
minimap_ecobicis=MiniMap(toggle_display=True,tile_layer="Stamen Terrain")
m_ecobicis.add_child(minimap_ecobicis)
m_ecobicis

#Agregar leyenda de capacidad:
from folium.plugins import FloatImage
image_file= 'https://raw.githubusercontent.com/MartinRodriguez93/imagenes/main/Leg_map_ecobicis.png?token=ASZ3KZGKTC3VWCD2VDLIJU3AODWC4'
FloatImage(image_file, bottom=31, left=87.5).add_to(m_ecobicis)
m_ecobicis
