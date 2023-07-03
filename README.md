# django-vue3-note
## 說明Django與Vue之間程式上的撰寫差異
## Django
### import
from django.db import models

### Vue3
### import
import { ref } from 'vue';  
//1.import在前，
2.import進來的功能名稱用{}包起來，
3.引用來源名稱用''單引號包起來
4.結束要用分號;作結束。
