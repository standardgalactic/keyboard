# Keyboard Layout Classifier

This project employs a neural network to determine whether a given word is better optimized for typing on a **QWERTY** or **Dvorak** keyboard layout.

## What It Does

![](flutter-01.jpg)

You input a word like `hello`, and the model predicts whether it's more "natural" to type on a QWERTY or Dvorak keyboard, based on:

- **Home row usage**
- **Common bigram patterns**
- **Letter frequency**
- **Finger movement cost**

The model outputs a score between 0 and 1:
- Values near 1 mean the word is likely Dvorak
- Values near 0 suggest QWERTY
- Scores around 0.5 indicate uncertainty

Example:

```bash
$ python classify_word.py typewriter
```

Output:
```
'typewriter' → QWERTY (0.00)
```

## Model

- **Framework:** TensorFlow + Keras
- **Trained on:** Word feature dataset generated from the Top 10,000 English Words
- **Accuracy:** ~81% on the test set
- **Architecture:** 2 hidden layers (32 → 16 units), sigmoid output
- **CPU-only:** This model was trained and runs using CPU (no CUDA/GPU)

## Features Used for Training

For each word, we calculate:

| Feature         | Description                                         |
|-----------------|-----------------------------------------------------|
| home_row_score  | Bonus points for letters on the home row            |
| bigram_bonus    | Bonus for specific letter pairs (e.g., ck, th)      |
| frequency_score | Weighted sum of character frequency in English      |
| movement_cost   | Total key travel distance across the keyboard       |

These features were calculated separately for QWERTY and Dvorak layouts to generate training labels.

## 🛠 How to Use

![](flutter-02.jpg)

1. Clone the repo
2. Ensure you have Python 3 and install dependencies:
   ```bash
   pip install tensorflow scikit-learn joblib
   ```
3. Run the classifier on any word:
   ```bash
   python classify_word.py hello
   ```

To classify multiple words:
```bash
for word in hello world typewriter function zoo; do \
    python3 classify_word.py "$word" 2>/dev/null; \
    done
```

Output:
```
'hello' → Dvorak (0.60)
'world' → QWERTY (0.47)
'typewriter' → QWERTY (0.00)
'function' → QWERTY (0.00)
'zoo' → QWERTY (0.30)
```


# Broader Fan Language

The Telegraph-courier (Kenosha, Wis.), November 22, 1923
https://www.loc.gov/resource/sn85040310/1923-11-22/ed-1/?sp=8&st=image&r=-0.343,-0.142,1,1.447,0

And:
The Western Sentinel (Winston-Salem,) Oct. 24, 1922, edition 1 / Page 13
https://newspapers.digitalnc.org/lccn/sn85042327/1922-10-24/ed-1/seq-13/

See also:
https://www.sothebys.com/en/articles/the-secret-language-of-fans#:~:text=Nowadays%2C%20it%20is%20widely%20believed,suggestion%20to%20just%20be%20friends.

https://www.whalingmuseum.org/online_exhibits/fans/language.html#:~:text=Letting%20the%20fan%20rest%20on,%22No.%22


