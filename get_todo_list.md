
https://huxulm.github.io/lc-rating/#/zen
```
from bs4 import BeautifulSoup
import random

with open('element.txt', 'r', encoding='utf-8') as f:
    line = f.readline()

soup = BeautifulSoup(line, 'html.parser')
tbody = soup.find('tbody')
trs = tbody.find_all('tr')

out_rows = []

for tr in trs:
    # 找 <td></td> 标签
    tds = tr.find_all('td')
    problem_a = tds[2].find_all('a')[0]
    problem_source = tds[1].find_all('a')[0]
    problem_index = tds[0].text.strip()
    problem_title = problem_a.text.strip()
    problem_href = problem_a.get('href')
    problem_source_text = problem_source.text.strip()
    problem_source_href = problem_source.get('href')
    out_rows.append(f"- [ ] {problem_index}: \t [{problem_title}]({problem_href}) \n")

random.seed(2023)
random.shuffle(out_rows)
# new_list = [(i + random.random()*len(out_rows), x) for i, x in enumerate(out_rows)]
# out_rows = [x for i, x in sorted(new_list)]

k = 20
split_len = int(len(out_rows) / k)
print(split_len)
for i in range(k):
    with open(f"todo_list_{i}.md", "w", encoding='utf-8') as f:
        f.write(f'# ToDo list {i}: \n\n\n')
        end = (i+1)*split_len if i < k-1 else len(out_rows)
        for row in out_rows[i*split_len:end]:
            f.write(row)
```
