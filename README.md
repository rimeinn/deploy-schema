# deploy-schema
一个 GitHub Actions 工作流，用来部署 Rime 方案并打包给 [fcitx5-rime.js](https://github.com/rimeinn/fcitx5-rime.js)。

## 参数说明

* user-recipe-list：必填，每行一个 [plum](https://github.com/rime/plum) 可识别的配方。
* shared-recipe-list：可选，每行一个在共享目录预装的配方，默认为 essay 和 prelude。
* schema-list 可选，每行一个 default.custom.yaml 中启用的方案。当且仅当您的方案自带 default.yaml 时省略此项。
* package-items：可选，打包进 zip 的项，默认为 build。

## 产物
/tmp/deploy-schema/artifact.zip，可直接用于 `loadZip`。

## 示例

[雾凇拼音](https://github.com/iDvel/rime-ice)

```yaml
- name: Build rime-ice
  uses: rimeinn/deploy-schema@master
  with:
    # rime-ice README.md 给出的配方
    user-recipe-list: |-
      iDvel/rime-ice:others/recipes/full
    # 作为简体方案，它不依赖 prelude 和 essay，因此用空值覆盖默认值
    shared-recipe-list:
    # 因为自带 default.yaml，省略 schema-list
    # 除了 build 外，这几项也是运行时要用到的
    package-items: |-
      build
      lua
      opencc
      custom_phrase.txt
```

[朙月拼音](https://github.com/rime/rime-luna-pinyin)和[五笔画](https://github.com/rime/rime-stroke)
```yaml
- name: Build luna-pinyin and stroke
  uses: rimeinn/deploy-schema@master
  with:
    user-recipe-list: |-
      luna-pinyin
      stroke
    # 依赖 prelude 和 essay，省略 shared-recipe-list
    schema-list: |-
      luna_pinyin
      stroke
    # 只打包 build，省略 package-items
```
