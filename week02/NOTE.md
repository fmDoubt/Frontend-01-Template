
2. 第二题 理解为将Unicode编码转为UTF-8格式
```
let Utf8 = function(data) {
    let decimal = data.charCodeAt(0).toString(10)
    let binary = data.charCodeAt(0).toString(2)
    let len = binary.length
    if(0 <= decimal <= 127){
        return '0' + addZero(binary, 6)
    }else if(128 <= decimal <= 2047){
        if(len < 6){
            return '1100000010' + addZero(binary, 6)
        }else if(len === 6){
            return '1100000010' + binary
        }else {
            return '110'+ addZero(binary.slice(0, -6), 5)+
              '10' + binary.slice(-6)
        }
    }else if(2048 <= decimal <= 65535){
        if(len <= 6){
            return '11100000100000010' + addZero(binary, 6)
        }else if(6 < len <= 12){
            return '1110000010'+ addZero(binary.slice(0, -6), 5) +
            '10' + binary.slice(-6)
        }else {
            return '1110' + addZero(binary.slice(0, -12), 4) +
              '10' + binary.slice(-12, -6) +
              '10' + binary.slice(-6)
        }
    }else if(65536 <= decimal <= 1114111){
        if(len <= 6){
            return '11110000100000001000000010' + addZero(binary, 6)
        }else if(6 < len <= 12){
            return '111100001000000010' + addZero(binary.slice(0, -6), 6) +
              '10' + binary.slice(-6)
        }else if(12 < len <= 18){
            return '1111000010' + addZero(binary.slice(0, -12), 6) +
              '10' + binary.slice(-12, -6) +
              '10' + binary.slice(-6)
        }else {
            return '11110' + addZero(binary.slice(0, -18), 3) +
              '10' + binary.slice(-18, -12) +
              '10' + binary.slice(-12, -6) +
              '10' + binary.slice(-6)
        }
    }else {
        return false
    }
}

function addZero(str, len){
    while(str.length < len){
        str = '0' + str
    }
    return str
}
```
