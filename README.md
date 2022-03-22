# dev_9
# 1
```
from pprint import pprint

import requests

def get_id(name):
    req = "https://superheroapi.com/api/2619421814940190/search/" + name
    response = requests.get(req)
    response.raise_for_status()
    json_dict = response.json()
    results = json_dict["results"]
    for result in results:
        if result["name"] == name:
            id = result["id"]
            break
    return id

def get_intelligence(id):
    req = "https://superheroapi.com/api/2619421814940190/" + id
    response = requests.get(req)
    response.raise_for_status()
    json_dict = response.json()
    intelligence = int(json_dict["powerstats"]["intelligence"])
    return intelligence

if __name__ == '__main__':
    list_superhero = ["Hulk", "Captain America", "Thanos"]
    final_intelligence = 0
    for hero in list_superhero:
        current_intelligence = get_intelligence(get_id(hero))
        if current_intelligence > final_intelligence:
            final_intelligence = current_intelligence
            winner = hero
    print(f'Победитель – {winner}')
```
Вывод  
```
Победитель – Thanos
```
# 2
```
from pprint import pprint
import pathlib
from pathlib import Path
import os

import requests

KEY = "OAuth AQAAAAAWTeeYAADLW0-rjvA3UE1bqNEUIgDXDu4"
HEADERS = {"Authorization": KEY}


class YaUploader:
    def __init__(self, token: str):
        self.token = token

    def upload(self, file_path):
        """Метод загруджает файл file_path на яндекс диск"""
        response = requests.get("https://cloud-api.yandex.net/v1/disk/resources/upload",
                                params={"path": os.path.basename(file_path), "overwrite": "true"},
                                headers=HEADERS
                                )
        response.raise_for_status()
        data = response.json()
        # pprint(data)
        href = data["href"]
        with open(file_path, "rb") as f:
            put_response = requests.put(href, files={"file": f}, headers=HEADERS)
            put_response.raise_for_status()
            # pprint(put_response.text)
        return 'Файл успешно загружен'


if __name__ == '__main__':
    uploader = YaUploader(KEY)
    dir_path = Path.home()
    f_path = Path(dir_path, "Downloads", "1.docx")
    print(f_path)
    result = uploader.upload(f_path)
    print(result)
```

    
