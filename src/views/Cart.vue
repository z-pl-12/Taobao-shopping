<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

// 优惠券数据
const coupons = ref([
  {
    id: 1,
    title: '新用户专享券',
    description: '满200元减30元',
    amount: 30,
    minAmount: 200,
    deadline: '2026-04-30',
    status: 'unused'
  },
  {
    id: 2,
    title: '春季大促券',
    description: '满300元减50元',
    amount: 50,
    minAmount: 300,
    deadline: '2026-05-15',
    status: 'unused'
  },
  {
    id: 3,
    title: '品牌活动券',
    description: '满500元减100元',
    amount: 100,
    minAmount: 500,
    deadline: '2026-03-31',
    status: 'expired'
  },
  {
    id: 4,
    title: '限时特惠券',
    description: '满100元减20元',
    amount: 20,
    minAmount: 100,
    deadline: '2026-04-10',
    status: 'used'
  }
])

// 优惠券弹窗显示状态
const showCouponModal = ref(false)

// 切换优惠券弹窗显示
const toggleCouponModal = () => {
  showCouponModal.value = !showCouponModal.value
}

// 使用优惠券
const useCoupon = (coupon) => {
  if (coupon.status === 'unused') {
    console.log('使用优惠券:', coupon.title)
  }
}

// 模拟购物车数据
const cartItems = ref([
  {
    id: 1,
    shop: '天猫 忆梦十月旗舰店',
    title: '学院风假两件针织连衣裙',
    size: 'M',
    color: '黑色（不含腰链）',
    price: 89,
    discountedPrice: 78.3,
    quantity: 1,
    image: '/src/assets/衣服1.png',
    selected: false,
    hasCoupon: true,
    officialDiscount: true,
    priceDrop: false
  },
  {
    id: 2,
    shop: '天猫 cenzci旗舰店',
    title: '新款条纹放置蓝色针织连衣两件套',
    size: '45/55/M',
    color: '吉吉黄',
    price: 151.8,
    discountedPrice: 132.8,
    quantity: 2,
    image: '/src/assets/衣服2.png',
    selected: false,
    hasCoupon: true,
    officialDiscount: true,
    priceDrop: false
  },
  {
    id: 3,
    shop: '淘宝 TAOJ时尚女装',
    title: '洋气别针设计上衣半身裙两件套',
    size: 'L',
    color: '套装',
    price: 218.9,
    discountedPrice: 191.9,
    quantity: 1,
    image: '/src/assets/衣服3.png',
    selected: false,
    hasCoupon: false,
    officialDiscount: true,
    priceDrop: true
  },
  {
    id: 4,
    shop: '天猫 菲娅妮旗舰店',
    title: '灰色西装套装裙春秋大学生搭配外套',
    size: 'XL',
    color: 'K51919',
    price: 278,
    discountedPrice: 264.1,
    quantity: 1,
    image: '/src/assets/衣服4.png',
    selected: false,
    hasCoupon: true,
    officialDiscount: false,
    priceDrop: false
  },
  {
    id: 5,
    shop: '淘宝 时尚女装馆',
    title: '韩版宽松针织衫长袖毛衣',
    size: 'M',
    color: '米白色',
    price: 168,
    discountedPrice: 148.5,
    quantity: 1,
    image: '/src/assets/衣服1.png',
    selected: false,
    hasCoupon: true,
    officialDiscount: true,
    priceDrop: false
  },
  {
    id: 6,
    shop: '天猫 优衣库官方旗舰店',
    title: '男士纯棉T恤短袖',
    size: 'L',
    color: '白色',
    price: 99,
    discountedPrice: 79.2,
    quantity: 2,
    image: '/src/assets/衣服2.png',
    selected: false,
    hasCoupon: false,
    officialDiscount: true,
    priceDrop: true
  },
  {
    id: 7,
    shop: '淘宝 衣香丽影',
    title: '复古格子西装外套女春秋',
    size: 'M',
    color: '卡其色',
    price: 299,
    discountedPrice: 259.9,
    quantity: 1,
    image: '/src/assets/衣服3.png',
    selected: false,
    hasCoupon: true,
    officialDiscount: false,
    priceDrop: false
  },
  {
    id: 8,
    shop: '天猫 ONLY官方旗舰店',
    title: '高腰阔腿牛仔裤女',
    size: 'S',
    color: '浅蓝色',
    price: 359,
    discountedPrice: 287.2,
    quantity: 1,
    image: '/src/assets/衣服4.png',
    selected: false,
    hasCoupon: true,
    officialDiscount: true,
    priceDrop: false
  }
])

// 猜你喜欢商品数据
const recommendedProducts = [
  {
    id: 1,
    title: '耐克 男子篮球鞋 Air Zoom BB NXT',
    price: 188.5,
    sales: 500,
    discount: 25,
    image: '/src/assets/猜你喜欢1.png'
  },
  {
    id: 2,
    title: '仙女裙 女秋季新款',
    price: 268.0,
    sales: 1200,
    discount: 18,
    image: '/src/assets/猜你喜欢2.png'
  },
  {
    id: 3,
    title: '安踏 女款运动鞋',
    price: 145.9,
    sales: 800,
    discount: 30,
    image: '/src/assets/猜你喜欢3.png'
  },
  {
    id: 4,
    title: '可爱水杯',
    price: 329.5,
    sales: 300,
    discount: 22,
    image: '/src/assets/猜你喜欢4.png'
  },
  {
    id: 5,
    title: '天空蓝套装女夏季新款',
    price: 199.9,
    sales: 1500,
    discount: 15,
    image: '/src/assets/猜你喜欢5.png'
  },
  {
    id: 6,
    title: '多巴胺裤子女新款特色',
    price: 258.8,
    sales: 600,
    discount: 28,
    image: '/src/assets/猜你喜欢6.png'
  },
  {
    id: 7,
    title: '泡泡玛特 盲盒系列',
    price: 178.0,
    sales: 900,
    discount: 20,
    image: '/src/assets/猜你喜欢7.png'
  },
  {
    id: 8,
    title: '特色多巴胺套装',
    price: 388.9,
    sales: 450,
    discount: 35,
    image: '/src/assets/猜你喜欢8.png'
  }
]

// 全选状态
const selectAll = ref(false)

// 当前选中的标签
const activeTab = ref('all')

// 切换标签
const switchTab = (tab) => {
  activeTab.value = tab
}

// 计算属性：根据选中的标签筛选商品
const filteredCartItems = computed(() => {
  if (activeTab.value === 'all') {
    return cartItems.value
  } else if (activeTab.value === 'official') {
    return cartItems.value.filter(item => item.officialDiscount)
  } else if (activeTab.value === 'priceDrop') {
    return cartItems.value.filter(item => item.priceDrop)
  }
  return cartItems.value
})

// 计算选中商品的总价
const totalPrice = computed(() => {
  return filteredCartItems.value
    .filter(item => item.selected)
    .reduce((total, item) => total + item.discountedPrice * item.quantity, 0)
    .toFixed(2)
})

// 计算选中商品的数量
const selectedCount = computed(() => {
  return filteredCartItems.value.filter(item => item.selected).length
})

// 弹窗相关
const showSpecModal = ref(false)
const currentItem = ref(null)

// 规格选项
const sizeOptions = ['S', 'M', 'L', 'XL']
const colorOptions = [
  { name: '黑色（含腰链）', value: '黑色（含腰链）' },
  { name: '黑色（不含腰链）', value: '黑色（不含腰链）' },
  { name: '吉吉黄', value: '吉吉黄' },
  { name: '套装', value: '套装' },
  { name: 'K51919', value: 'K51919' }
]

// 处理全选/取消全选
const handleSelectAll = (event) => {
  const isChecked = event.target.checked
  console.log('全选框状态变化:', isChecked)
  selectAll.value = isChecked
  cartItems.value.forEach(item => {
    item.selected = isChecked
  })
  console.log('全选后商品选中状态:', cartItems.value.map(item => ({ id: item.id, selected: item.selected })))
}

// 打开规格弹窗
const openSpecModal = (item) => {
  currentItem.value = { ...item }
  showSpecModal.value = true
}

// 关闭规格弹窗
const closeSpecModal = () => {
  showSpecModal.value = false
  currentItem.value = null
}

// 确认修改规格
const confirmSpecChange = () => {
  if (currentItem.value) {
    const index = cartItems.value.findIndex(item => item.id === currentItem.value.id)
    if (index !== -1) {
      cartItems.value[index] = { ...currentItem.value }
    }
  }
  closeSpecModal()
}

// 处理单个商品选择
const handleSelectItem = (item) => {
  // 检查是否所有商品都被选中
  selectAll.value = filteredCartItems.value.every(i => i.selected)
}

// 增加商品数量
const increaseQuantity = (item) => {
  item.quantity++
}

// 减少商品数量
const decreaseQuantity = (item) => {
  if (item.quantity > 1) {
    item.quantity--
  }
}

// 删除商品
const deleteItem = (id) => {
  if (!id) {
    // 点击全选旁边的删除按钮
    const selectedItems = filteredCartItems.value.filter(item => item.selected)
    if (selectedItems.length === 0) {
      alert('请先选择要删除的商品')
      return
    }
    if (confirm('确定全部删除吗？')) {
      const selectedIds = selectedItems.map(item => item.id)
      cartItems.value = cartItems.value.filter(item => !selectedIds.includes(item.id))
    }
  } else {
    // 删除单个商品
    cartItems.value = cartItems.value.filter(item => item.id !== id)
  }
}

// 移入收藏夹
const moveToFavorites = (id) => {
  console.log('移入收藏夹:', id)
}

// 结算
const checkout = () => {
  if (selectedCount.value === 0) {
    alert('请选择要结算的商品')
    return
  }
  console.log('结算:', cartItems.value.filter(item => item.selected))
}

// 搜索功能
const searchQuery = ref('')
const searchPlaceholders = ref(['女生正装西服', '多巴胺传达', '记事本'])
const currentPlaceholderIndex = ref(0)

// 轮播搜索框占位文字
setInterval(() => {
  currentPlaceholderIndex.value = (currentPlaceholderIndex.value + 1) % searchPlaceholders.value.length
}, 2000)

const handleSearch = () => {
  console.log('搜索:', searchQuery.value)
}

// 倒计时相关
const countdownTime = ref(93 * 60 + 35) // 初始时间: 93分35秒转换为秒

// 格式化倒计时时间
const formatCountdown = (seconds) => {
  const hours = Math.floor(seconds / 3600)
  const minutes = Math.floor((seconds % 3600) / 60)
  const secs = seconds % 60
  const hoursStr = String(hours).padStart(2, '0')
  const minutesStr = String(minutes).padStart(2, '0')
  const secsStr = String(secs).padStart(2, '0')
  return `<span style="display: inline-block; background-color: #ff69b4; color: #fff; padding: 0px 3px; border-radius: 1px; font-size: 16px; font-weight: bold; min-width: 32px; text-align: center;">${hoursStr}</span>:<span style="display: inline-block; background-color: #ff69b4; color: #fff; padding: 0px 3px; border-radius: 1px; font-size: 16px; font-weight: bold; min-width: 32px; text-align: center;">${minutesStr}</span>:<span style="display: inline-block; background-color: #ff69b4; color: #fff; padding: 0px 3px; border-radius: 1px; font-size: 16px; font-weight: bold; min-width: 32px; text-align: center;">${secsStr}</span>`
}

// 计算属性：格式化后的倒计时时间
const formattedCountdown = computed(() => formatCountdown(countdownTime.value))

// 启动倒计时
onMounted(() => {
  const timer = setInterval(() => {
    if (countdownTime.value > 0) {
      countdownTime.value--
    }
  }, 1000)

  // 组件卸载时清除定时器
  onUnmounted(() => {
    clearInterval(timer)
  })
})

// 跳转到首页
const goToHome = () => {
  router.push('/')
}
</script>

<template>
  <div class="cart">
    <!-- 顶部导航栏 -->
    <header class="header">
      <div class="container-fluid">
        <!-- 右侧导航 -->
        <div class="right-nav flex-center justify-end">
          <span class="nav-item">淘宝首页</span>
          <span class="nav-item">已买到的宝贝</span>
          <span class="nav-item">我的淘宝</span>
          <span class="nav-item">购物车 4</span>
          <span class="nav-item">收藏夹</span>
          <span class="nav-item">免费开店</span>
          <span class="nav-item">千牛卖家中心</span>
          <span class="nav-item">帮助中心</span>
        </div>
      </div>
    </header>

    <!-- 购物车标题 -->
    <div class="cart-header">
      <div class="container-fluid flex-center">
        <img src="/src/assets/淘宝-copy.png" alt="淘宝" style="width: 100px; height: 30px; margin-right: 2px;" />
        <h1 style="color: #ff4400; font-size: 24px; font-weight: bold; cursor: pointer; margin: 0; padding: 10px 0;">购物车</h1>
      </div>
    </div>

    <!-- 购物车内容 -->
    <div class="cart-content">
      <div class="cart-main-wrapper">
        <!-- 左侧商品列表 -->
        <div class="cart-items">
          <!-- 分类标签 -->
          <div class="cart-tabs">
            <span :class="['tab', { active: activeTab === 'all' }]" @click="switchTab('all')">
              <img src="/src/assets/全部商品-copy.png" alt="全部商品" style="width: 20px; height: 20px; margin-right: 5px; vertical-align: middle;" />
              全部商品({{ cartItems.length }})
            </span>
            <span class="tab" @click="toggleCouponModal" style="cursor: pointer;">
              <img src="/src/assets/消费券.png" alt="消费券" style="width: 20px; height: 20px; margin-right: 5px; vertical-align: middle;" />
              消费券
            </span>
            <span :class="['tab', { active: activeTab === 'official' }]" @click="switchTab('official')">
              <img src="/src/assets/官方立减.png" alt="官方立减" style="width: 20px; height: 20px; margin-right: 5px; vertical-align: middle;" />
              官方立减
            </span>
            <span :class="['tab', { active: activeTab === 'priceDrop' }]" @click="switchTab('priceDrop')">
              <img src="/src/assets/降价.png" alt="降价" style="width: 20px; height: 20px; margin-right: 5px; vertical-align: middle;" />
              降价
            </span>
          </div>

          <!-- 操作栏 -->
          <div class="cart-actions">
            <div class="action-left">
              <input type="checkbox" :checked="selectAll" @change="handleSelectAll" style="accent-color: #DCDCDC !important;" />
              <span>全选</span>
              <span class="action-btn" @click="moveToFavorites">移入收藏</span>
              <span class="action-btn" @click="deleteItem">删除</span>
            </div>
            <div class="action-right">
              <select class="input">
                <option>分类</option>
              </select>
              <select class="input">
                <option>状态</option>
                <option>🔥即将售罄</option>
                <option>失效</option>
              </select>
              <div class="search-input-wrapper">
                <input type="text" placeholder="搜索" class="input" />
                <img src="/src/assets/搜索 (1).png" alt="搜索" class="search-icon-small" />
              </div>
              <button class="btn" @click="handleSearch"></button>
            </div>
          </div>

          <!-- 消费券提示 -->
          <div class="coupon-alert">
            <span>🧧 您有 2 张共计140元消费券，可尽快使用</span>
            <span class="countdown">距结束 <span class="countdown-number" v-html="formattedCountdown"></span></span>
          </div>

        <!-- 商品列表 -->
<div class="cart-item" v-for="item in filteredCartItems" :key="item.id">
  <div class="item-checkbox">
    <input type="checkbox" v-model="item.selected" @change="handleSelectItem(item)" style="accent-color: #DCDCDC !important;" />
  </div>
  <div class="item-content">
    <div class="item-shop">
      <span class="shop-tag">天猫</span>
      <span>{{ item.shop }}</span>
      <img src="/src/assets/icon.png" alt="店标" class="shop-icon" />
    </div>
    <div class="item-main">
      <div class="item-image-checkbox">
        <input type="checkbox" v-model="item.selected" style="accent-color: #DCDCDC !important;" />
      </div>
      <div class="item-image">
        <img :src="item.image" :alt="item.title" />
        <div class="image-hover-effect">
          <div class="preview-image">
            <img :src="item.image" :alt="item.title" />
          </div>
          <button class="find-similar-btn">🔍 找同款</button>
        </div>
      </div>
      <div class="item-content-wrapper">
        <!-- 第一列：标题、标签、支付方式、促销信息 -->
        <div class="item-column item-column-1">
          <div class="title-section">
            <span class="title-tag">38焕新周</span>
            <span class="item-title-text">{{ item.title }}</span>
          </div>
          <button class="payment-btn" title="信用卡支付">信用卡支付</button>
          <div class="promotion-buttons">
            <button class="promotion-btn" title="官方立减10.7元 大促价保 假一赔四 极速退款">官方立减  大促价保  假一赔四...</button>
          </div>
        </div>
        
        <!-- 第二列：规格信息 -->
        <div class="item-column item-column-2">
          <div class="spec-info" @mouseenter="true" @mouseleave="false">
            <div class="spec-content">
              <div class="spec-item">尺码: {{ item.size }}</div>
              <div class="spec-item">颜色分类: {{ item.color }}</div>
            </div>
            <button class="modify-btn" @click="openSpecModal(item)">修改</button>
          </div>
        </div>
        
        <!-- 第三列：价格信息 -->
        <div class="item-column item-column-3">
          <div class="column-item discounted-price">券后价 ¥{{ item.discountedPrice }}</div>
          <div class="column-item original-price">¥{{ item.price }}</div>
        </div>
        
        <!-- 第四列：数量调整 -->
        <div class="item-column item-column-4">
          <div class="column-item">
            <div class="item-quantity">
              <button class="btn quantity-btn" @click="decreaseQuantity(item)">-</button>
              <input type="number" v-model="item.quantity" class="input quantity-input" />
              <button class="btn quantity-btn" @click="increaseQuantity(item)">+</button>
            </div>
          </div>
        </div>
        
        <!-- 第五列：操作按钮 -->
        <div class="item-column item-column-5">
          <div class="column-item action-btn" @click="moveToFavorites(item.id)">移入收藏</div>
          <div class="column-item action-btn" @click="deleteItem(item.id)">删除</div>
        </div>
      </div>
    </div>
  </div>
</div>

          <!-- 猜你喜欢 -->
          <div class="recommended-section">
            <h3>猜你喜欢</h3>
            <div class="recommended-products">
              <div class="recommended-item" v-for="product in recommendedProducts" :key="product.id">
                <img :src="product.image" :alt="product.title" class="recommended-img" />
                <div class="recommended-title">{{ product.title }}</div>
                <div class="recommended-discount">官方立减{{ product.discount }}%</div>
                <div class="recommended-price-sales">
                  <span class="recommended-price">¥{{ product.price }}</span>
                  <span class="recommended-sales">{{ product.sales }}人购买</span>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- 右侧搜索和结算区域 -->
        <div class="checkout-sidebar">
          <!-- 搜索栏 -->
          <div class="sidebar-search">
            <input 
              type="text" 
              v-model="searchQuery" 
              :placeholder="searchPlaceholders[currentPlaceholderIndex]" 
              class="input sidebar-search-input"
            />
            <div class="search-icon">
              <img src="/src/assets/相机.png" alt="相机" />
            </div>
            <button class="btn btn-primary sidebar-search-btn">搜淘宝</button>
          </div>
          
          <!-- 结算明细模块 -->
          <div class="checkout-module">
            <div class="checkout-header">
              <h3>结算明细</h3>
            </div>
            <div class="checkout-content">
              <div class="checkout-empty" v-if="selectedCount === 0">
                <div class="empty-icon">🛒</div>
                <div class="empty-text">选择商品查看实际支付价格</div>
              </div>
              <div v-else class="checkout-items">
                <div class="checkout-item" v-for="item in cartItems.filter(i => i.selected)" :key="item.id">
                  <div class="checkout-item-title">{{ item.title }}</div>
                  <div class="checkout-item-info">
                    <span class="checkout-item-price">¥{{ item.discountedPrice }}</span>
                    <span class="checkout-item-quantity">x{{ item.quantity }}</span>
                  </div>
                </div>
              </div>
              <div class="checkout-total" v-if="selectedCount > 0">
                <span>合计:</span>
                <span class="total-price">¥{{ totalPrice }}</span>
              </div>
              <button class="btn btn-primary checkout-btn" @click="checkout" :disabled="selectedCount === 0">
                结算 ({{ selectedCount }})
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 规格选择弹窗 -->
    <div v-if="showSpecModal" class="spec-modal-overlay" @click="closeSpecModal">
      <div class="spec-modal" @click.stop>
        <div class="spec-modal-header">
          <h3>选择规格</h3>
          <button class="close-btn" @click="closeSpecModal">×</button>
        </div>
        <div class="spec-modal-content">
          <div class="spec-image">
            <img :src="currentItem?.image" :alt="currentItem?.title" />
          </div>
          <div class="spec-options">
            <div class="spec-option-group">
              <h4>颜色分类</h4>
              <div class="option-buttons">
                <button 
                  v-for="color in colorOptions" 
                  :key="color.value"
                  :class="['option-btn', { active: currentItem?.color === color.value }]"
                  @click="currentItem.color = color.value"
                >
                  {{ color.name }}
                </button>
              </div>
            </div>
            <div class="spec-option-group">
              <h4>尺码</h4>
              <div class="option-buttons">
                <button 
                  v-for="size in sizeOptions" 
                  :key="size"
                  :class="['option-btn', { active: currentItem?.size === size }]"
                  @click="currentItem.size = size"
                >
                  {{ size }}
                </button>
              </div>
            </div>
          </div>
        </div>
        <div class="spec-modal-footer">
          <button class="btn cancel-btn" @click="closeSpecModal">取消</button>
          <button class="btn confirm-btn" @click="confirmSpecChange">确定</button>
        </div>
      </div>
    </div>
  </div>

  <!-- 优惠券弹窗 -->
  <div class="coupon-modal-overlay" v-if="showCouponModal" @click="toggleCouponModal">
    <div class="coupon-modal" @click.stop>
      <div class="coupon-modal-header">
        <h3>我的优惠券</h3>
        <button class="close-btn" @click="toggleCouponModal">×</button>
      </div>
      <div class="coupon-modal-body">
        <div class="coupon-tabs">
          <span class="coupon-tab active">全部</span>
          <span class="coupon-tab">未使用</span>
          <span class="coupon-tab">已使用</span>
          <span class="coupon-tab">已过期</span>
        </div>
        <div class="coupon-list">
          <div 
            v-for="coupon in coupons" 
            :key="coupon.id" 
            :class="['coupon-item', coupon.status]"
          >
            <div class="coupon-left">
              <div class="coupon-amount">¥{{ coupon.amount }}</div>
              <div class="coupon-condition">满{{ coupon.minAmount }}元可用</div>
            </div>
            <div class="coupon-right">
              <div class="coupon-title">{{ coupon.title }}</div>
              <div class="coupon-description">{{ coupon.description }}</div>
              <div class="coupon-deadline">有效期至: {{ coupon.deadline }}</div>
              <div class="coupon-status">
                <span v-if="coupon.status === 'unused'" class="status-badge unused">未使用</span>
                <span v-else-if="coupon.status === 'used'" class="status-badge used">已使用</span>
                <span v-else-if="coupon.status === 'expired'" class="status-badge expired">已过期</span>
              </div>
            </div>
            <button 
              v-if="coupon.status === 'unused'" 
              class="coupon-use-btn" 
              @click="useCoupon(coupon)"
            >
              立即使用
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cart {
  min-height: 100vh;
  background-color: #fff;
  margin: 0;
  padding: 0;
  width: 100%;
}

/* 顶部导航栏 */
.header {
  background-color: #fff;
  border-bottom: 1px solid #e0e0e0;
  padding: 10px 0;
  position: sticky;
  top: 0;
  z-index: 1000;
  width: 100%;
  left: 0;
  right: 0;
}

.logo h1 {
  margin: 0;
}

.search-box {
  flex: 1;
  max-width: 600px;
  margin: 0 20px;
}

.search-input {
  flex: 1;
  height: 36px;
  border-radius: 18px 0 0 18px;
  border-right: none;
  border: 1px solid #e0e0e0;
  outline: none;
  padding: 0 15px;
}

.search-input:focus {
  border-color: #ff4400;
}

.search-btn {
  height: 36px;
  border-radius: 0 18px 18px 0;
  padding: 0 20px;
  background-color: #ff4400;
  color: #fff;
  border: none;
  cursor: pointer;
}

.right-nav {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 20px;
}

.right-nav .nav-item {
  cursor: pointer;
  font-size: 14px;
}

.right-nav .nav-item:hover {
  color: #ff4400;
}

/* 购物车标题 */
.cart-header {
  background-color: #fff;
  border-bottom: none;
  padding: 4px 20px;
  display: block;
}

/* 容器样式 */
.container-fluid {
  width: 100%;
  max-width: 100%;
  margin: 0;
  padding: 0 20px;
  box-sizing: border-box;
}

.cart-header-section {
  max-width: 1400px;
  margin: 0 auto;
}

.flex-center {
  display: flex;
  align-items: center;
}

.justify-end {
  justify-content: flex-end;
}

/* 购物车内容 */
.cart-content {
  padding: 0;
  background-color: #fff;
  border-top: none;
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
}

/* 购物车主容器 */
.cart-main-wrapper {
  display: flex;
  width: 100%;
  max-width: 100%;
  padding: 0 20px 50px 20px;
  gap: 20px;
}

/* 左侧商品列表 */
.cart-items {
  flex: 1;
  background-color: #fff;
  padding: 5px;
  margin-right: 0;
  border-radius: 0;
  min-width: 0;
}

/* 分类标签 */
.cart-tabs {
  display: flex;
  gap: 20px;
  margin-bottom: 5px;
}

.tab {
  padding: 5px 10px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.3s ease;
}

.tab:hover {
  color: #ff4400;
}

.tab.active {
  color: #ff4400;
  font-weight: bold;
  border-bottom: 2px solid #ff4400;
}

/* 操作栏 */
.cart-actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 3px 0;
  border-bottom: 1px solid #e0e0e0;
  margin-bottom: 4px;
}

.action-left {
  display: flex;
  align-items: center;
  gap: 15px;
}

.action-left input[type="checkbox"] {
  width: 20px;
  height: 20px;
  margin-right: 5px;
  vertical-align: middle;
  accent-color: #DCDCDC !important;
}

/* 商品列表 - 核心布局修正 */
.cart-item {
  display: flex;
  padding: 5px 0;
  border-bottom: 1px solid #f0f0f0;
  align-items: flex-start;
}

.item-checkbox {
  margin-right: 15px;
  display: flex;
  align-items: center;
  min-width: 20px;
}

.item-checkbox input[type="checkbox"] {
  width: 20px;
  height: 20px;
  vertical-align: middle;
  margin: 0;
  accent-color: #DCDCDC !important;
}

/* 图片左侧的勾选框 */
.item-image-checkbox {
  margin-right: 10px;
  display: flex;
  align-items: center;
}

.item-image-checkbox input[type="checkbox"] {
  width: 20px;
  height: 20px;
  vertical-align: middle;
  accent-color: #DCDCDC !important;
}

.action-btn {
  cursor: pointer;
  font-size: 14px;
  color: #666;
}

.action-btn:hover {
  color: #ff4400;
}

/* 全选旁边的操作按钮 */
.action-left .action-btn {
  padding: 4px 12px;
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  background-color: white;
  display: inline-block;
  transition: all 0.3s ease;
}

.action-left .action-btn:hover {
  color: #ff4400;
  border-color: #ff4400;
}

.action-right {
  display: flex;
  align-items: center;
  gap: 10px;
}

.action-right .input {
  width: 100px;
  height: 30px;
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  padding: 0 10px;
  outline: none;
}

/* 搜索框包装器 */
.search-input-wrapper {
  position: relative;
  display: inline-block;
}

.search-input-wrapper .input {
  width: 150px;
  padding-right: 35px;
}

/* 搜索图标 */
.search-icon-small {
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  width: 16px;
  height: 16px;
  cursor: pointer;
  pointer-events: none;
}

/* 消费券提示 */
.coupon-alert {
  background-color: #fff3e0;
  border: 1px solid #ec462d;
  border-radius: 4px;
  padding: 10px;
  margin-bottom: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.coupon-alert span:first-child {
  color: #e83f36;
  font-size: 20px;
  font-weight: bold;
}

.countdown {
  color: #ff4400;
  font-weight: bold;
  font-size: 16px;
}

.countdown-number {
  display: inline-flex;
  gap: 4px;
  align-items: center;
}

.countdown-number :deep(.countdown-unit) {
  display: inline-block;
  background-color: #ed4f44;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 16px;
  font-weight: bold;
  min-width: 45px;
  text-align: center;
}

/* 商品列表 - 核心布局修正 */
.cart-item {
  display: flex;
  padding: 20px 0;
  border-bottom: 1px solid #f0f0f0;
}

.item-checkbox {
  margin-right: 15px;
  display: flex;
  align-items: flex-start;
  margin-top: 5px;
}

.item-checkbox input[type="checkbox"] {
  width: 20px;
  height: 20px;
  accent-color: #DCDCDC !important;
}

/* 图片左侧的勾选框 */
.item-image-checkbox {
  margin-right: 10px;
  display: flex;
  align-items: center;
}

.item-image-checkbox input[type="checkbox"] {
  width: 20px;
  height: 20px;
  accent-color: #DCDCDC !important;
}

.item-content {
  flex: 1;
}

.item-shop {
  font-size: 14px;
  color: #666;
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  gap: 5px;
}

.shop-tag {
  color: #ff4400;
  font-weight: bold;
}

.shop-icon {
  width: 16px;
  height: 16px;
  vertical-align: middle;
}

/* 主内容区：图片+五列布局 */
.item-main {
  display: flex;
  gap: 15px;
  align-items: center;
}

.item-image {
  flex-shrink: 0;
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: visible;
}

.item-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 4px;
  transition: all 0.3s ease;
}

/* 鼠标悬停效果 */
.image-hover-effect {
  position: absolute;
  top: -10px;
  left: 110px;
  background-color: white;
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  padding: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  z-index: 100;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.item-image:hover .image-hover-effect {
  opacity: 1;
  visibility: visible;
}

.preview-image {
  width: 180px;
  height: 180px;
  overflow: hidden;
  border-radius: 4px;
}

.preview-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.find-similar-btn {
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 5px 10px;
  font-size: 12px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 5px;
  transition: all 0.3s ease;
}

.find-similar-btn:hover {
  background-color: #ff4400;
  color: white;
  border-color: #ff4400;
}

.item-content-wrapper {
  flex: 1;
  display: flex;
  gap: 15px;
  min-width: 0;
}

/* 五列布局 */
.item-column {
  display: flex;
  flex-direction: column;
  gap: 5px;
  min-width: 0;
}

/* 第一列宽度设置 */
.item-column-1 {
  flex: 2;
  min-width: 0;
}

/* 其他列宽度设置 */
.item-column-2, .item-column-3, .item-column-4, .item-column-5 {
  flex: 1;
  min-width: 0;
}

/* 第一列：标题、标签等 */
.item-column-1 .column-item {
  margin-bottom: 5px;
}

.title-section {
  display: flex;
  align-items: center;
  gap: 5px;
  margin-bottom: 5px;
  flex-wrap: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.item-column-1 .title-tag {
  background: none;
  color: #DC143C;
  font-size: 16px;
  font-weight: bold;
  padding: 0;
  border-radius: 0;
}

.item-column-1 .item-title-text {
  font-size: 14px;
  color: #333;
  line-height: 1.4;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.item-column-1 .payment-method {
  font-size: 12px;
  color: #666;
}

/* 促销按钮横向排列 */
.promotion-buttons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin-top: 5px;
}

.promotion-btn {
  font-size: 12px;
  color: #ff4400;
  background: none;
  border: none;
  padding: 1px 5px;
  cursor: pointer;
  position: relative;
  white-space: nowrap;
}

.promotion-btn:hover {
  background-color: #fff3e0;
  border-radius: 2px;
}

/* 悬停下拉效果 - 下拉菜单形式 */
.promotion-btn:hover::after {
  content: '官方立减10.7元\A大促价保\A假一赔四\A极速退款';
  position: absolute;
  top: 100%;
  left: 0;
  background-color: white;
  color: black;
  padding: 10px;
  border-radius: 4px;
  font-size: 12px;
  white-space: pre-wrap;
  z-index: 1000;
  margin-top: 5px;
  min-width: 120px;
  line-height: 1.5;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  border: 1px solid #e0e0e0;
}

.promotion-btn:hover::before {
  content: '';
  position: absolute;
  top: 100%;
  left: 10px;
  border-width: 5px;
  border-style: solid;
  border-color: transparent transparent white transparent;
  z-index: 1001;
  margin-top: -5px;
}

/* 第二列：规格信息 */
.item-column-2 .column-item {
  font-size: 12px;
  color: #999;
  margin-bottom: 5px;
}

/* 规格信息悬停效果 */
.spec-info {
  position: relative;
  display: inline-block;
  padding: 5px;
  border-radius: 4px;
  transition: all 0.3s ease;
}

.spec-info:hover {
  outline: 1px dashed #999;
}

.spec-content {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.spec-item {
  font-size: 12px;
  color: #999;
}

.modify-btn {
  position: absolute;
  top: -5px;
  right: -5px;
  background-color: #ff4400;
  color: white;
  font-size: 10px;
  padding: 2px 5px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  opacity: 0;
  transition: opacity 0.3s ease;
  white-space: nowrap;
}

.spec-info:hover .modify-btn {
  opacity: 1;
}

/* 规格选择弹窗 */
.spec-modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.spec-modal {
  background-color: white;
  border-radius: 8px;
  width: 500px;
  max-width: 90%;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
}

.spec-modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
  border-bottom: 1px solid #e0e0e0;
}

.spec-modal-header h3 {
  margin: 0;
  font-size: 16px;
  font-weight: bold;
  color: #333;
}

.close-btn {
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  color: #999;
}

.spec-modal-content {
  padding: 20px;
  display: flex;
  gap: 20px;
}

.spec-image {
  flex-shrink: 0;
}

.spec-image img {
  width: 100px;
  height: 100px;
  object-fit: cover;
  border-radius: 4px;
}

.spec-options {
  flex: 1;
}

.spec-option-group {
  margin-bottom: 20px;
}

.spec-option-group h4 {
  margin: 0 0 10px 0;
  font-size: 14px;
  font-weight: bold;
  color: #333;
}

.option-buttons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.option-btn {
  padding: 5px 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: white;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.3s ease;
}

.option-btn:hover {
  border-color: #ff4400;
  color: #ff4400;
}

.option-btn.active {
  border-color: #ff4400;
  color: #ff4400;
  background-color: #fff3e0;
}

.spec-modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  padding: 15px 20px;
  border-top: 1px solid #e0e0e0;
  background-color: #f9f9f9;
  border-radius: 0 0 8px 8px;
}

.cancel-btn {
  padding: 8px 20px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: white;
  cursor: pointer;
  font-size: 14px;
}

.confirm-btn {
  padding: 8px 20px;
  border: none;
  border-radius: 4px;
  background-color: #ff4400;
  color: white;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
}

/* 第三列：价格信息 */
.item-column-3 .column-item {
  margin-bottom: 5px;
}

.item-column-3 .discounted-price {
  font-size: 16px;
  font-weight: bold;
  color: #ff4400;
}

.item-column-3 .original-price {
  font-size: 12px;
  color: #999;
  text-decoration: line-through;
}

/* 第四列：数量调整 */
.item-column-4 .column-item {
  margin-bottom: 5px;
  display: block !important;
}

.item-column-4 .item-quantity {
  display: flex !important;
  align-items: center !important;
  gap: 0 !important;
  border: 1px solid #ddd !important;
  border-radius: 4px !important;
  overflow: hidden !important;
}

.item-column-4 .quantity-btn {
  width: 28px !important;
  height: 28px !important;
  display: flex !important;
  align-items: center !important;
  justify-content: center !important;
  background-color: #f5f5f5 !important;
  border: none !important;
  border-radius: 0 !important;
  cursor: pointer !important;
  padding: 0 !important;
  font-size: 16px !important;
}

.item-column-4 .quantity-btn:first-child {
  border-right: 1px solid #ddd !important;
}

.item-column-4 .quantity-btn:last-child {
  border-left: 1px solid #ddd !important;
}

.item-column-4 .quantity-input {
  width: 50px !important;
  height: 28px !important;
  text-align: center !important;
  border: none !important;
  border-radius: 0 !important;
  outline: none !important;
  padding: 0 !important;
  font-size: 14px !important;
}

/* 第五列：操作按钮 */
.item-column-5 .column-item {
  margin-bottom: 10px;
  cursor: pointer;
  font-size: 14px;
  color: #666;
}

.item-column-5 .column-item:hover {
  color: #ff4400;
}

/* 猜你喜欢 */
.recommended-section {
  margin-top: 30px;
}

.recommended-section h3 {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 15px;
  color: #333;
}

.recommended-products {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 15px;
}

.recommended-item {
  background-color: #f9f9f9;
  border-radius: 6px;
  padding: 8px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
}

.recommended-item:hover {
  background-color: #e8e8e8;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.recommended-img {
  width: 150px;
  height: 150px;
  object-fit: cover;
  border-radius: 6px;
  margin-bottom: 6px;
}

.recommended-title {
  font-size: 14px;
  line-height: 1.4;
  margin-bottom: 3px;
  color: #333;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.recommended-discount {
  font-size: 13px;
  color: #ff4400;
  margin-bottom: 3px;
}

.recommended-price-sales {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
}

.recommended-price {
  font-size: 16px;
  font-weight: bold;
  color: #ff4400;
}

.recommended-sales {
  font-size: 13px;
  color: #999;
}

/* 右侧结算区域 */
.checkout-sidebar {
  width: 300px;
  background-color: #fff;
  padding: 20px;
  height: fit-content;
  position: sticky;
  top: 100px;
  border-radius: 0;
  border-left: 1px solid #f0f0f0;
}

.checkout-header h3 {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 20px;
  color: #333;
}

.checkout-info {
  margin-bottom: 20px;
  text-align: center;
  color: #666;
  font-size: 14px;
}

.checkout-total {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding: 10px 0;
  border-top: 1px solid #f0f0f0;
  border-bottom: 1px solid #f0f0f0;
}

.checkout-total span:first-child {
  font-size: 14px;
  color: #333;
}

.total-price {
  font-size: 20px;
  font-weight: bold;
  color: #ff4400;
}

.checkout-btn {
  width: 100%;
  height: 40px;
  font-size: 16px;
  font-weight: bold;
  border-radius: 4px;
  background-color: #ff4400;
  color: #fff;
  border: none;
  cursor: pointer;
}

.payment-btn {
  font-size: 12px;
  color: #666;
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  display: inline;
}

.payment-btn:hover {
  text-decoration: underline;
  color: #ff4400;
}

/* 侧边栏搜索栏 */
.sidebar-search {
  display: flex;
  align-items: center;
  gap: 0;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  overflow: visible;
  height: 40px;
  padding: 0;
  border: 1px solid #ddd;
  width: 100%;
}

.sidebar-search-input {
  flex: 1;
  border-radius: 8px 0 0 8px;
  border: none;
  outline: none;
  padding: 0 16px;
  font-size: 14px;
  height: 40px;
  line-height: 40px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  background: transparent;
}

.sidebar-search-input:focus {
  border: none;
}

/* 相机图标 */
.search-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 10px;
  cursor: pointer;
  height: 100%;
  min-width: 40px;
  position: relative;
  z-index: 1;
}

.search-icon img {
  width: 24px;
  height: 24px;
  object-fit: contain;
  display: block;
}

.search-icon:hover {
  opacity: 0.7;
}

.sidebar-search-btn {
  border-radius: 0 8px 8px 0;
  padding: 0 20px;
  background-color: #ff4400;
  color: #fff;
  border: none;
  cursor: pointer;
  font-weight: bold;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  white-space: nowrap;
  letter-spacing: normal;
}

/* 结算模块 */
.checkout-module {
  background-color: #fff;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  width: 315px;
}

.checkout-header h3 {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 12px;
  color: #333;
}

/* 空购物车状态 */
.checkout-empty {
  text-align: center;
  padding: 15px 0;
  margin-bottom: 12px;
}

.empty-icon {
  font-size: 36px;
  color: #e0e0e0;
  margin-bottom: 8px;
}

.empty-text {
  font-size: 14px;
  color: #ff4400;
  margin-bottom: 12px;
}

.checkout-total {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
  padding: 8px 0;
  border-top: 1px solid #f0f0f0;
  border-bottom: 1px solid #f0f0f0;
}

.checkout-total span:first-child {
  font-size: 14px;
  color: #333;
}

.total-price {
  font-size: 20px;
  font-weight: bold;
  color: #ff4400;
}

/* 结算明细商品列表 */
.checkout-items {
  margin-bottom: 12px;
  max-height: 200px;
  overflow-y: auto;
}

.checkout-item {
  padding: 8px 0;
  border-bottom: 1px solid #f0f0f0;
}

.checkout-item:last-child {
  border-bottom: none;
}

.checkout-item-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.checkout-item-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.checkout-item-price {
  font-size: 16px;
  font-weight: bold;
  color: #ff4400;
}

.checkout-item-quantity {
  font-size: 14px;
  color: #999;
}

.checkout-btn {
  width: 100%;
  height: 45px;
  font-size: 16px;
  font-weight: bold;
  border-radius: 4px;
  background-color: #ff4400;
  color: #fff;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
}

.checkout-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.checkout-btn:hover:not(:disabled) {
  background-color: #ff6600;
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(255, 68, 0, 0.3);
}

/* 底部版权信息 */
.footer {
  margin-top: 50px;
  padding: 30px 0;
  border-top: 1px solid #f0f0f0;
  font-size: 12px;
  color: #999;
  text-align: center;
}

.footer-links {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.footer-link {
  cursor: pointer;
}

.footer-link:hover {
  color: #ff4400;
}

.footer-copyright {
  margin-top: 20px;
}

/* 优惠券弹窗 */
.coupon-modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.coupon-modal {
  background-color: #fff;
  border-radius: 8px;
  width: 800px;
  max-height: 600px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
}

.coupon-modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  border-bottom: 1px solid #f0f0f0;
  background: linear-gradient(135deg, #ff4400, #ff6600);
  color: #fff;
}

.coupon-modal-header h3 {
  margin: 0;
  font-size: 20px;
  font-weight: bold;
}

.coupon-modal-header .close-btn {
  background: none;
  border: none;
  font-size: 32px;
  color: #fff;
  cursor: pointer;
  padding: 0;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.2s ease;
}

.coupon-modal-header .close-btn:hover {
  transform: scale(1.2);
}

.coupon-modal-body {
  padding: 20px;
  max-height: 500px;
  overflow-y: auto;
}

.coupon-tabs {
  display: flex;
  gap: 20px;
  padding: 10px 0;
  border-bottom: 2px solid #f0f0f0;
  margin-bottom: 20px;
}

.coupon-tab {
  padding: 8px 16px;
  cursor: pointer;
  font-size: 15px;
  color: #666;
  position: relative;
  transition: all 0.3s ease;
}

.coupon-tab.active {
  color: #ff4400;
  font-weight: bold;
}

.coupon-tab.active::after {
  content: '';
  position: absolute;
  bottom: -12px;
  left: 0;
  right: 0;
  height: 2px;
  background-color: #ff4400;
}

.coupon-tab:hover {
  color: #ff4400;
}

.coupon-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.coupon-item {
  display: flex;
  background: linear-gradient(135deg, #fff5f0, #fff);
  border: 2px dashed #ffcccc;
  border-radius: 8px;
  padding: 16px;
  position: relative;
  transition: all 0.3s ease;
}

.coupon-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(255, 68, 0, 0.15);
  border-color: #ff4400;
}

.coupon-item.used {
  background: linear-gradient(135deg, #f5f5f5, #fafafa);
  border-color: #ccc;
  opacity: 0.6;
}

.coupon-item.expired {
  background: linear-gradient(135deg, #f0f0f0, #f5f5f5);
  border-color: #ddd;
  opacity: 0.5;
}

.coupon-left {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 0 20px;
  border-right: 2px dashed #ffcccc;
  margin-right: 16px;
  min-width: 100px;
}

.coupon-amount {
  font-size: 32px;
  font-weight: bold;
  color: #ff4400;
  line-height: 1;
}

.coupon-amount::before {
  content: '¥';
  font-size: 16px;
  margin-right: 2px;
}

.coupon-condition {
  font-size: 12px;
  color: #999;
  margin-top: 4px;
}

.coupon-right {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.coupon-title {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-bottom: 6px;
}

.coupon-description {
  font-size: 14px;
  color: #666;
  margin-bottom: 6px;
}

.coupon-deadline {
  font-size: 12px;
  color: #999;
}

.coupon-status {
  margin-top: 8px;
}

.status-badge {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: bold;
}

.status-badge.unused {
  background-color: #ffe6e6;
  color: #ff4400;
}

.status-badge.used {
  background-color: #e6e6e6;
  color: #999;
}

.status-badge.expired {
  background-color: #f0f0f0;
  color: #ccc;
}

.coupon-use-btn {
  position: absolute;
  right: 16px;
  top: 50%;
  transform: translateY(-50%);
  padding: 8px 20px;
  background: linear-gradient(135deg, #ff4400, #ff6600);
  color: #fff;
  border: none;
  border-radius: 20px;
  font-size: 14px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
}

.coupon-use-btn:hover {
  background: linear-gradient(135deg, #ff6600, #ff8844);
  transform: translateY(-50%) scale(1.05);
  box-shadow: 0 4px 12px rgba(255, 68, 0, 0.3);
}
</style>