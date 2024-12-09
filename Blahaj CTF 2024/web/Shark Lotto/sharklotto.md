# sharklotto

We are given this website with the objective to obtain 13371337 moneyz:

![image](https://github.com/user-attachments/assets/94002c70-1340-4b51-b53e-bc380cb25db8)

Seems simple enough. 1/16 chance of getting 10x the money... It's obvious we need to somehow get more money, bypassing this system.

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

From here, there doesn't seem to be checks on the backend on how much money we have. We can exploit this

![image](https://github.com/user-attachments/assets/705152dc-3673-4ea6-846b-3cb88b7bb94f)

Luckily, Burpsuite let's us change the request, allowing us to change the money value sent to the server to 13371337, and receiving the flag in the request.

`blahaj{d0n7_7rus7_th3_cl13nt}`
