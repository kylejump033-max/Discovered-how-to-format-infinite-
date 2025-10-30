# on AI unit (Python)
def on_message(msg):
    data = json.loads(msg.payload)
    features = extract_features(data)
    score = model.predict(features)  # TFLite run
    publish("ai/unit15/results", 
    {"node":data["node"], "score":score})
    # sends telemetry to MQTT broker
import time, json
import paho.mqtt.client as mqtt
node_id = "Drog-68-004"
broker = "192.168.1.10"
topic = f"drog/{node_id}/telemetry"

client = mqtt.Client(client_id=node_id)
client.tls_set()  # set certs or use secure broker config
client.username_pw_set(username="device", password="jwt-token-or-password")
client.connect(broker, 8883)

def read_sensors():
    # stub - replace with actual sensor reads
    return {"accel":[0,0,1],"mic_level":-42,"battery":88}

while True:
    payload = {
      "node": node_id,
      "ts": int(time.time()*1000),
      "code_chain": "0134579101110987654321000101",
      "sensors": read_sensors()
    }
    client.publish(topic, json.dumps(payload), qos=1)
    time.sleep(10)
