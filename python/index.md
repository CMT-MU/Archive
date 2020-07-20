- (2020.7.20) python docstring を doxygen で処理する
  1. pip install doxypypy   [doxypypy](https://github.com/Feneric/doxypypy)
  1. パスの通ったところに py_filter をおく
        #!/bin/bash
        doxypypy -a -c $1
  1. chmod +x py_filter
  1. Doxyfile の項目に以下を記述
        FILTER_PATTERNS       = *.py=py_filter
