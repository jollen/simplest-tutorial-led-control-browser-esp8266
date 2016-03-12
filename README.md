# simplest-tutorial-turn-led-on-browser-esp8266

## Step 1:

Run the code on NodeMCU (ESP8266)

```
-- Select IO - GPIO0 
outpin=3  
gpio.mode(outpin,gpio.OUTPUT)  
gpio.write(outpin,gpio.LOW)

function power(stat)  
  if stat=="ON"  then gpio.write(outpin,gpio.HIGH) 
    return end
  if stat=="OFF" then gpio.write(outpin,gpio.LOW) 
    return end
end

-- Print IP address
ip = wifi.sta.getip()  
print(ip)

-- Configure the ESP as a station (client)
wifi.setmode(wifi.STATION)  
wifi.sta.config("jollenchen", "qqqqqqqq")  
wifi.sta.autoconnect(1)

-- Create a server
-- and set 30s time out for a inactive client
sv = net.createServer(net.TCP, 30)

-- Server listen on 80
-- Print HTTP headers to console
sv:listen(80,function(c)  
    c:on("receive", function(conn, payload)
        print(payload)

        if (string.find(payload, "POST /power/on") ~= nil) then
            power("ON")
        elseif (string.find(payload, "POST /power/off") ~= nil) then
            power("OFF")
        end

        conn:send("HTTP/1.1 200 OK\n\n")
        conn:close()
    end)
end)  
```

## Step 2

Call ```/power/on``` and ```/power/off``` APIs by curl.

```
$ curl -X POST http://192.168.1.1/power/on
```

You need to replace the IP address with yours.
