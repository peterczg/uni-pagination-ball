#### 这是一个分页球，本身不提供分页功能，只起展示作用，可拖拽

##### installation
`npm install uni-pagination-ball --save`  or  `yarn add uni-pagination-ball`


##### import

`import PaginationBall from 'uni-pagination-ball'`


`components:{ 'pagination-ball':PaginationBall }`


##### use

```
<pagination-ball
   @end="handleEnd"
   :page-size="pageSize"
   :page-num="pageNum"
   :total-size="totalSize" 
   :is-end.sync="isEnd" 
    tip="到底了哦">
</pagination-ball>

<!-- 
 传入pageSize，pageNum和totalSize

 并且绑定is-end默认为false

 end事件是翻到最后一页的时候的自定义事件

 点击小球，可以看到totalSize  
 -->
  ```


**Tip：小球可拖拽哦**
  