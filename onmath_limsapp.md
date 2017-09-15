## 关于 onmath_limsapp 的基本功能与 API 说明

### 数据的导入说明
onmath_limsapp 导入规则的设计原则是: 保证在同一个项目下的 OMID 与 sample_id 必须具有唯一性。
- import_send_sample

将上传得到的送样结果导入到数据库，具体实现分为有无 OMID:
- 若上传结果存在 OMID 则首先在数据库中搜索当前项目是否上传数据中存在数据库中不存在的 OMID 当存在数据库中没有的 OMID 时报错。否则将存在相同的 OMID 数据标记删除后（可理解为更新OMID）将全部上传数据导入数据库。
- 如果不存在 OMID 则会自动分配生成 OMID。然后将全部数据导入到数据库。


- import_quality_check

导入规则基本同送样，具体实现也分为有无 OMID。

- 上传结果有 OMID 和送样基本一致，当上传结果没有 OMID 时会根据送样结果的 sample_id 去自动匹配 OMID 如果存在不存在的 sample_id 系统会提示不存在相应的 sample_id 而报错。然后再具体根据上传的 sample_id 去和当前项目下已经存在（如果存在的话）的 sample_id 对比
如果数据库中已经存在了当前 sample_id 则会标记删除后将全部上传数据导入到数据库。


- import_build_lib

过程见 quality_check。

- import_upmachine

导入规则基本同质检，具体实现也分为有无 OMID。

- 基本过程和 quality_check 一致，只是当上传数据存在所在地时系统会判断当前为第一次上传上机结果，然后直接将全部数据导入。否则将数据视为数据库已经存在的数据，先根据 sample_id 标记删除数据库已经存在的数据然后再将上传数据全部导入到数据库。这样的结果会让上机部分的所在地一直显示为第一次上传的结果。

- import_downmachine

导入规则基本同质检，具体实现也分为有无 OMID。

- import_return_sample

由于返样是针对于全部的项目所以当上传返样结果到数据库中时会立即更新送样结果的所在地。这个过程不依赖于具体项目，而是以 sample_id 为 key 进行的更新。
