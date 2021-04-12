Python challenge 0

![calc](https://user-images.githubusercontent.com/73337770/114421566-56a16f80-9bad-11eb-98a4-07bc06401e96.jpg)

    print(pow(2,38))
    
 result: 274877906944
 
 Python challenge 1
 
 ![map](https://user-images.githubusercontent.com/73337770/114422218-fb23b180-9bad-11eb-8d2f-f332cc044ecd.jpg)
 
    myString = "g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj."

    myOtherString = "map"
   
    outtab = "cdefghijklmnopqrstuvwxyzab"
    intab = "abcdefghijklmnopqrstuvwxyz"

    trantab = myString.maketrans(intab,outtab)
    print(myOtherString.translate(trantab))
    
  result: ocr
  
  Python challenge 2
  
  ![ocr](https://user-images.githubusercontent.com/73337770/114422624-581f6780-9bae-11eb-9e30-7913cdfc58c7.jpg)
  
    f = open('cha0.txt')
    
    x = f.read()
    x.count("a")

    alphabet = "abcdefghijklmnopqrstuvwxyz"

    for letter in alphabet:
    if x.count(letter) == 1: 
        print(letter)

    #"aeilqtuy" = "equality"
    
  result: equality
  
  Python challenge 3 
  
  ![bodyguard](https://user-images.githubusercontent.com/73337770/114423094-c106df80-9bae-11eb-8fa7-d5d052460e75.jpg)

    import urllib.request
    import re

    html = urllib.request.urlopen("http://www.pythonchallenge.com/pc/def/equality.html").read().decode()
    data = re.findall("<!--(.*)-->", html, re.DOTALL)[-1]
    print("".join(re.findall("[^A-Z]+[A-Z]{3}([a-z])[A-Z]{3}[^A-Z]+", data)))
    
  result: linkedlist
  
  Python challenge 4
  
  ![chainsaw](https://user-images.githubusercontent.com/73337770/114425068-a0d82000-9bb0-11eb-9a86-ad8862cc2648.jpg)

    from urllib.request import urlopen
    import re

    uri = "http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=%s"

    num = "12345"
    num = str(16044/2)

    pattern = re.compile("and the next nothing is (\d+)")

    while True:
    content = urlopen(uri % num).read().decode('utf-8')
    print(content)
    match = pattern.search(content)
    if match == None:
        break
    num = match.group(1) 
    
  result: peak.html
  
  Python challenge 5
  
  ![peakhell](https://user-images.githubusercontent.com/73337770/114425594-20fe8580-9bb1-11eb-8c62-dfed21d02794.jpg)

    from urllib.request import urlopen
    import pickle

    h = urlopen("http://www.pythonchallenge.com/pc/def/banner.p")
    data = pickle.load(h)

    for line in data:
    print("".join([k * v for k, v in line]))  
    
  result: channel
  
  Python challenge 6
  
  ![channel](https://user-images.githubusercontent.com/73337770/114425883-6622b780-9bb1-11eb-8953-6b0ba4e0ef85.jpg)
  
    import zipfile, re

    f = zipfile.ZipFile("channel.zip")
    print(f.read("readme.txt").decode("utf-8"))

    num = '90052'

    comments = []

    while True:
    content = f.read(num + ".txt").decode("utf-8")
    comments.append(f.getinfo(num + ".txt").comment.decode("utf-8"))
    print(content)
    match = re.search("Next nothing is (\d+)", content)
    if match == None:
        break
    num = match.group(1)

    print("".join(comments))
    
   result: oxygen
   
   Python challenge 7
   
   ![oxygen](https://user-images.githubusercontent.com/73337770/114430045-ecd99380-9bb5-11eb-966b-44ec5db3b580.png)

    pip install pypng
    
    from urllib.request import urlopen
    import png, re

    response = urlopen("http://www.pythonchallenge.com/pc/def/oxygen.png")

    (w, h, rgba, dummy) = png.Reader(response).read()

    it = list(rgba)
    mid = int(h / 2)
    l = len(it[mid])
    res = [chr(it[mid][i]) for i in range(0, l, 4*7) if it[mid][i] == it[mid][i + 1] == it[mid][i + 2]]
    print("".join(map(chr, map(int, re.findall("\d+", "".join(res))))))
    
   result: integrity
   


 

