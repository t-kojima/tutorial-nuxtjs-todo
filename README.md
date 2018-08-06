# example-migration-from-jinja2

jinja2 で辛い問題

- 良いフォーマッタがない。`{% %}` の位置が何か変
- JavaScript にデータが渡し辛い。
  - jinja2 の問題ではないが、DOM を介したくない
  - `function({{ data }})` という渡し方もできなくはないが、エディタはエラーを吐く（実際は動く）
