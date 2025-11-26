<template>
  <div class="leisure-page">
    <!-- Header Component -->
    <LeisureHeader />
    
    <!-- Categories Component -->
    <LeisureCategories @categoryChange="onCategoryChange" :activeCategory="activeCategory" />
    
    <!-- Category Content -->
    <component :is="activeCategoryComponent" 
               :recommendedGoods="recommendedGoods"
               :massageGoods="massageGoods"
               :bathGoods="bathGoods"
               :ktvGoods="ktvGoods" />
  </div>
</template>

<script>
import LeisureHeader from './components/LeisureHeader.vue';
import LeisureCategories from './components/LeisureCategories.vue';
import RecommendCategory from './components/RecommendCategory.vue';
import MassageCategory from './components/MassageCategory.vue';
import BathCategory from './components/BathCategory.vue';
import KTVCategory from './components/KTVCategory.vue';

export default {
  name: 'LeisurePage',
  components: {
    LeisureHeader,
    LeisureCategories,
    RecommendCategory,
    MassageCategory,
    BathCategory,
    KTVCategory
  },
  data() {
    return {
      activeCategory: 'recommend',
      recommendedGoods: [
        {
          id: 1,
          name: '高级按摩套餐',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.8,
          sales: 1234,
          distance: '1.2km',
          price: 299,
          originalPrice: 399,
          tags: ['按摩', '放松', '套餐']
        },
        {
          id: 2,
          name: '豪华KTV包房',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.7,
          sales: 890,
          distance: '800m',
          price: 399,
          originalPrice: 599,
          tags: ['KTV', '聚会', '豪华']
        },
        {
          id: 3,
          name: '精品汗蒸体验',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.9,
          sales: 1560,
          distance: '1.5km',
          price: 129,
          originalPrice: 199,
          tags: ['汗蒸', '养生', '精品']
        },
        {
          id: 4,
          name: '养生足浴套餐',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.6,
          sales: 950,
          distance: '1.0km',
          price: 199,
          originalPrice: 299,
          tags: ['足浴', '养生', '套餐']
        }
      ],
      massageGoods: [
        {
          id: 1,
          name: '高级按摩套餐',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.8,
          sales: 1234,
          distance: '1.2km',
          price: 299,
          originalPrice: 399,
          tags: ['按摩', '放松', '套餐']
        },
        {
          id: 2,
          name: '养生足浴套餐',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.6,
          sales: 950,
          distance: '1.0km',
          price: 199,
          originalPrice: 299,
          tags: ['足浴', '养生', '套餐']
        },
        {
          id: 3,
          name: '泰式按摩体验',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.7,
          sales: 850,
          distance: '2.5km',
          price: 349,
          originalPrice: 499,
          tags: ['按摩', '泰式', '放松']
        }
      ],
      bathGoods: [
        {
          id: 1,
          name: '精品汗蒸体验',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.9,
          sales: 1560,
          distance: '1.5km',
          price: 129,
          originalPrice: 199,
          tags: ['汗蒸', '养生', '精品']
        },
        {
          id: 2,
          name: '温泉洗浴套餐',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.7,
          sales: 980,
          distance: '3.2km',
          price: 299,
          originalPrice: 499,
          tags: ['洗浴', '温泉', '养生']
        },
        {
          id: 3,
          name: '桑拿汗蒸体验',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.8,
          sales: 1120,
          distance: '2.0km',
          price: 149,
          originalPrice: 249,
          tags: ['汗蒸', '桑拿', '放松']
        }
      ],
      ktvGoods: [
        {
          id: 1,
          name: '豪华KTV包房',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.7,
          sales: 890,
          distance: '800m',
          price: 399,
          originalPrice: 599,
          tags: ['KTV', '聚会', '豪华']
        },
        {
          id: 2,
          name: '主题KTV体验',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.8,
          sales: 1020,
          distance: '1.8km',
          price: 299,
          originalPrice: 499,
          tags: ['KTV', '主题', '聚会']
        },
        {
          id: 3,
          name: '迷你KTV欢唱',
          image: 'https://via.placeholder.com/180x180',
          rating: 4.6,
          sales: 750,
          distance: '1.2km',
          price: 199,
          originalPrice: 299,
          tags: ['KTV', '迷你', '欢唱']
        }
      ]
    };
  },
  computed: {
    activeCategoryComponent() {
      switch(this.activeCategory) {
        case 'recommend':
          return 'RecommendCategory';
        case 'massage':
          return 'MassageCategory';
        case 'bath':
          return 'BathCategory';
        case 'ktv':
          return 'KTVCategory';
        default:
          return 'RecommendCategory';
      }
    }
  },
  methods: {
    onCategoryChange(category) {
      this.activeCategory = category;
    }
  }
};
</script>

<style lang="scss" scoped>
.leisure-page {
  background-color: #f5f5f5;
  min-height: 100vh;
}
</style>