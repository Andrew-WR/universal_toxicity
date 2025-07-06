import json

d = []
#prompts are from the real toxicity prompts dataset
with open("prompts.jsonl", "r", encoding="utf-8") as f:
    for i, line in enumerate(f, 1):
        if i > 5000:
          break
        try:
            obj = json.loads(line)
            d.append(obj)
        except json.JSONDecodeError as e:
            print(f"Line {i} has an error: {e}")

print(f"d[0]: {d[0]}")
dataset = []

with open('toprealtoxicity.txt', 'r') as f:
    lines = f.readlines()
    for line in lines:
        data = {'text': line.strip(), 'level': 'high'}
        dataset.append(data)


n = 2000 - len(dataset)
intervals = len(d) // (2 * n)

for i in range(0, n, intervals):
    #get each line
    line = d[i]['prompt']

    prompt = line['text']

    #check if max toxicity score: 0.3-0.6: medium, >0.6: high
    types = ["profanity","sexually_explicit","identity_attack","flirtation","threat","insult","severe_toxicity","toxicity"]
    m = -1
    for t in range(len(types)):
        c = line.get(types[t]) # Use .get() to handle missing keys and default to None

        if c is not None and m < c: # Add a check for None
            m = c
    if m >= 0.3 and m <= 0.6:
        data = {'text': prompt, 'level': 'medium'}
    elif m > 0.6:
        data = {'text': prompt, 'level': 'high'}
    #append into dataset
    if m != -1: # Only append if a valid toxicity score was found
        dataset.append(data)
print(f"length of dataset: {len(dataset)}")

with open('toxicity_dataset.json', 'w') as f:
    json.dump(dataset, f)
