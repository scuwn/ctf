# sharklotto

We are given this website:
(insert image here)

This is the main backend script that we need to exploit
```
@app.route('/spin', methods=['POST'])
def spin():
    bet_amount = request.json.get('bet')
    money = request.json.get('money')
    slotImage1 = random.randrange(0, 4)
    slotImage2 = random.randrange(0, 4)
    slotImage3 = random.randrange(0, 4)

    if (money >= 13371337):
        return jsonify({'flag': "blahaj{???}"})

    return jsonify({
        'slotImage1': slotImage1,
        'slotImage2': slotImage2,
        'slotImage3': slotImage3,
        'win': slotImage1 == slotImage2 and slotImage2 == slotImage3,
        'win_amt': bet_amount * 10 if slotImage1 == slotImage2 and slotImage2 == slotImage3 else 0
    })
```

It's a simple luck game. But it's heavily stacked against us. We cannot just rely on luck to solve this. However, there doesn't seem to be checks on the backend on how much money we have. We can exploit this

Luckily, Burpsuite let's us change the request, allowing us to change the money value sent to the server to 13371337, and receiving the flag in the request.
