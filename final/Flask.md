## Flask란..?
플라스크(Flask)는 파이썬으로 작성된 마이크로 웹 프레임워크의 하나이다.  
플라스크는 특별한 도구나 라이브러리가 필요 없기 때문에 마이크로 프레임워크라고 부른다.  

## 실행
```
python -m pip install -r requirements.txt
pip list
flask run
```  

### app.py
```
if __name__ == "__main__": 
    app.run(host='0.0.0.0', port='5000', debug=True)
# 반드시 host='0.0.0.0'가 설정되어어야함
```
