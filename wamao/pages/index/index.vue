<template>
  <div class="min-h-screen font-mono bg-gray-100">
    <!-- 顶部标题栏 -->
    <div class="bg-clay text-white py-3">
      <div class="container mx-auto flex justify-center">
        <h1 class="text-xl font-bold">云南瓦猫工艺制作坊</h1>
      </div>
    </div>

    <!-- 主体内容：竖版流程布局 -->
    <div class="container mx-auto py-4 px-2">
      <!-- 流程节点导航 -->
      <div class="flex flex-col items-center mb-6 relative">
        <!-- 中间曲线（流程线） -->
        <div class="absolute left-1/2 top-0 bottom-0 w-1 bg-clay -translate-x-1/2 z-0"></div>
        
        <!-- 节点1：原料制备 -->
        <div class="process-node" :data-step="1">
          <div class="node-dot" :class="{ 'bg-white ring-4 ring-clay': currentStep === 1 }"></div>
          <button 
            class="mt-2 px-3 py-1 rounded-full text-sm font-bold smooth-transition"
            :class="currentStep === 1 ? 'bg-clay text-white' : 'bg-gray-300 text-gray-700'"
            @click="changeStep(1)"
          >
            原料制备
          </button>
        </div>
        
        <!-- 节点2：基础塑形 -->
        <div class="process-node" :data-step="2">
          <div class="node-line"></div>
          <div class="node-dot" :class="{ 'bg-white ring-4 ring-clay': currentStep === 2 }"></div>
          <button 
            class="mt-2 px-3 py-1 rounded-full text-sm font-bold smooth-transition"
            :class="currentStep === 2 ? 'bg-clay text-white' : 'bg-gray-300 text-gray-700'"
            @click="changeStep(2)"
          >
            基础塑形
          </button>
        </div>
        
        <!-- 节点3：晾晒修整 -->
        <div class="process-node" :data-step="3">
          <div class="node-line"></div>
          <div class="node-dot" :class="{ 'bg-white ring-4 ring-clay': currentStep === 3 }"></div>
          <button 
            class="mt-2 px-3 py-1 rounded-full text-sm font-bold smooth-transition"
            :class="currentStep === 3 ? 'bg-clay text-white' : 'bg-gray-300 text-gray-700'"
            @click="changeStep(3)"
          >
            晾晒修整
          </button>
        </div>
        
        <!-- 节点4：高温烧制 -->
        <div class="process-node" :data-step="4">
          <div class="node-line"></div>
          <div class="node-dot" :class="{ 'bg-white ring-4 ring-clay': currentStep === 4 }"></div>
          <button 
            class="mt-2 px-3 py-1 rounded-full text-sm font-bold smooth-transition"
            :class="currentStep === 4 ? 'bg-clay text-white' : 'bg-gray-300 text-gray-700'"
            @click="changeStep(4)"
          >
            高温烧制
          </button>
        </div>
        
        <!-- 节点5：装饰赋色 -->
        <div class="process-node" :data-step="5">
          <div class="node-line"></div>
          <div class="node-dot" :class="{ 'bg-white ring-4 ring-clay': currentStep === 5 }"></div>
          <button 
            class="mt-2 px-3 py-1 rounded-full text-sm font-bold smooth-transition"
            :class="currentStep === 5 ? 'bg-clay text-white' : 'bg-gray-300 text-gray-700'"
            @click="changeStep(5)"
          >
            装饰赋色
          </button>
        </div>
      </div>

      <!-- 原料制备步骤 -->
      <div 
        id="step-1" 
        class="process-step bg-white rounded-lg p-4 card-shadow"
        v-if="currentStep === 1"
      >
        <h2 class="text-xl text-clay font-bold mb-3 text-center">原料制备：红土与细砂的在地配比</h2>
        <div class="flex flex-col gap-4">
          <div class="w-full">
            <h3 class="text-lg text-kiln font-bold mb-2">原料区</h3>
            <div class="grid grid-cols-2 gap-3">
              <div 
                class="material-item bg-clay text-white p-3 rounded-lg text-center" 
                data-material="红土" 
                draggable="true"
                @dragstart="handleDragStart"
              >
                <i class="fas fa-mountain text-2xl mb-1"></i>
                <p>红土 (70%)</p>
              </div>
              <div 
                class="material-item bg-sand text-kiln p-3 rounded-lg text-center" 
                data-material="细砂" 
                draggable="true"
                @dragstart="handleDragStart"
              >
                <i class="fas fa-palette text-2xl mb-1"></i>
                <p>细砂 (30%)</p>
              </div>
            </div>
          </div>
          <div class="w-full">
            <h3 class="text-lg text-kiln font-bold mb-2">配比容器</h3>
            <div 
              id="mixing-container" 
              class="bg-gray-200 min-h-[120px] rounded-lg flex flex-col items-center justify-center p-3"
              @dragover.prevent
              @drop="handleDrop"
            >
              <template v-if="clayCount + sandCount === 0">
                <i class="fas fa-flask text-4xl text-gray-400 mb-1"></i>
                <p class="text-gray-400 text-sm">拖拽红土和细砂到这里混合</p>
              </template>
              <template v-else>
                <div class="flex flex-col items-center justify-center">
                  <div 
                    class="w-10 h-10 bg-clay rounded-full mb-1" 
                    :style="{ width: `${(clayCount / 10) * 100}px`, height: `${(clayCount / 10) * 100}px` }"
                  ></div>
                  <div 
                    class="w-6 h-6 bg-sand rounded-full" 
                    :style="{ width: `${(sandCount / 10) * 100}px`, height: `${(sandCount / 10) * 100}px` }"
                  ></div>
                </div>
              </template>
              <div id="material-count" class="mt-2 text-xs text-gray-500">
                红土：{{ clayCount }} / 细砂：{{ sandCount }}
              </div>
            </div>
            <button 
              id="check-mix" 
              class="mt-3 w-full bg-clay text-white px-3 py-1.5 rounded-lg smooth-transition"
              :class="{ 'cursor-not-allowed opacity-50': !(clayCount === 7 && sandCount === 3) }"
              :disabled="!(clayCount === 7 && sandCount === 3)"
              @click="checkMix"
            >
              检查配比
            </button>
          </div>
        </div>
      </div>

      <!-- 基础塑形步骤 -->
      <div 
        id="step-2" 
        class="process-step bg-white rounded-lg p-4 card-shadow"
        v-if="currentStep === 2"
      >
        <h2 class="text-xl text-sand font-bold mb-3 text-center">基础塑形：小而敦实的纳财形态</h2>
        <div class="flex flex-col gap-4">
          <div class="w-full">
            <h3 class="text-lg text-kiln font-bold mb-2">瓦猫雏形</h3>
            <div id="cat-base" class="bg-gray-200 rounded-lg p-2 text-center">
              <img src="https://picsum.photos/150/150?random=1" alt="瓦猫基础雏形" class="w-full rounded-lg">
            </div>
          </div>
          <div class="w-full">
            <h3 class="text-lg text-kiln font-bold mb-2">部件选择区</h3>
            <div class="grid grid-cols-3 gap-2 mb-3">
              <div 
                class="part-item bg-white p-1 rounded-lg border-2 border-sand" 
                :data-part="part.part" 
                :data-type="part.type"
                v-for="(part, index) in parts" 
                :key="index"
                @click="selectPart(part.part, part.type)"
              >
                <img 
                  :src="`https://picsum.photos/60/60?random=${index + 2}`" 
                  :alt="`${part.part}样式${part.type}`" 
                  class="w-full rounded"
                >
              </div>
            </div>
            <button 
              id="free-draw" 
              class="w-full bg-sand text-kiln px-3 py-1.5 rounded-lg"
              @click="enterFreeDraw"
            >
              <i class="fas fa-pen-fancy mr-1"></i>自由创作
            </button>
          </div>
        </div>
      </div>

      <!-- 晾晒修整步骤 -->
      <div 
        id="step-3" 
        class="process-step bg-white rounded-lg p-4 card-shadow"
        v-if="currentStep === 3"
      >
        <h2 class="text-xl text-kiln font-bold mb-3 text-center">晾晒修整：阴干+细磨的耐心工序</h2>
        <div class="flex flex-col gap-4">
          <div class="w-full">
            <h3 class="text-lg text-kiln font-bold mb-2">阴干区</h3>
            <div id="drying-area" class="bg-gray-200 rounded-lg p-2 relative">
              <img src="https://picsum.photos/200/120?random=11" alt="阴干架" class="w-full rounded-lg">
              <div id="drying-timer" class="absolute top-2 right-2 bg-kiln text-white px-2 py-0.5 rounded-full text-xs">
                {{ dryingTimeText }}
              </div>
            </div>
          </div>
          <div class="w-full">
            <h3 class="text-lg text-kiln font-bold mb-2">修整工具</h3>
            <div class="grid grid-cols-2 gap-2">
              <button 
                class="tool-item p-2.5 rounded-lg text-center"
                :class="dryingCompleted ? 'bg-kiln text-white' : 'bg-gray-300'"
                data-tool="砂纸"
                @click="useTool('砂纸')"
                :disabled="!dryingCompleted"
              >
                <i class="fas fa-sandpaper text-2xl mb-1"></i>
                <p class="text-xs">砂纸</p>
              </button>
              <button 
                class="tool-item p-2.5 rounded-lg text-center"
                :class="dryingCompleted ? 'bg-kiln text-white' : 'bg-gray-300'"
                data-tool="刻刀"
                @click="useTool('刻刀')"
                :disabled="!dryingCompleted"
              >
                <i class="fas fa-knife text-2xl mb-1"></i>
                <p class="text-xs">刻刀</p>
              </button>
            </div>
            <div id="repair-result" class="mt-3 bg-gray-200 p-3 rounded-lg min-h-[80px] flex items-center justify-center">
              <p class="text-gray-400 text-sm" v-if="!repairResultText">阴干完成后请选择工具修整</p>
              <p class="text-kiln text-sm" v-else>{{ repairResultText }}</p>
            </div>
          </div>
        </div>
      </div>

      <!-- 高温烧制步骤 -->
      <div 
        id="step-4" 
        class="process-step bg-white rounded-lg p-4 card-shadow"
        v-if="currentStep === 4"
      >
        <h2 class="text-xl text-glaze font-bold mb-3 text-center">高温烧制：龙窑松烟的古朴釉色</h2>
        <div class="flex flex-col gap-4">
          <div class="w-full">
            <h3 class="text-lg text-kiln font-bold mb-2">龙窑</h3>
            <div id="kiln-area" class="bg-gray-800 rounded-lg p-2 relative">
              <img src="https://picsum.photos/200/120?random=12" alt="龙窑" class="w-full rounded-lg">
              <div 
                id="kiln-fire" 
                class="absolute bottom-2 left-1/2 transform -translate-x-1/2 bg-yellow-500 w-10 h-6 rounded-t-full smooth-transition"
                :class="{ 'opacity-0': !isFiring, 'opacity-100 animate-pulse': isFiring && !firingCompleted }"
              ></div>
            </div>
          </div>
          <div class="w-full">
            <h3 class="text-lg text-kiln font-bold mb-2">烧制结果</h3>
            <div id="glaze-result" class="bg-gray-200 rounded-lg p-2 min-h-[120px] flex items-center justify-center">
              <p class="text-gray-400 text-sm" v-if="!firingCompleted">将瓦猫放入龙窑开始烧制</p>
              <p class="text-gray-400 text-sm" v-else-if="firingCompleted && !selectedGlaze">选择喜欢的釉色</p>
              <div 
                id="glaze-colors" 
                class="grid grid-cols-3 gap-1.5 mt-2"
                v-else
              >
                <div 
                  class="glaze-color w-8 h-8 rounded-full bg-amber-500 cursor-pointer" 
                  data-color="amber"
                  @click="selectGlaze('amber')"
                ></div>
                <div 
                  class="glaze-color w-8 h-8 rounded-full bg-gray-600 cursor-pointer" 
                  data-color="gray"
                  @click="selectGlaze('gray')"
                ></div>
                <div 
                  class="glaze-color w-8 h-8 rounded-full bg-emerald-500 cursor-pointer" 
                  data-color="emerald"
                  @click="selectGlaze('emerald')"
                ></div>
              </div>
            </div>
          </div>
          <button 
            id="start-firing" 
            class="w-full bg-glaze text-white px-3 py-1.5 rounded-lg"
            @click="startFiring"
            :disabled="isFiring || firingCompleted"
          >
            <i class="fas fa-play mr-1"></i>开始烧制
          </button>
        </div>
      </div>

      <!-- 装饰赋色步骤 -->
      <div 
        id="step-5" 
        class="process-step bg-white rounded-lg p-4 card-shadow"
        v-if="currentStep === 5"
      >
        <h2 class="text-xl text-gray-700 font-bold mb-3 text-center">装饰赋色：素陶为本的极简点缀</h2>
        <div class="flex flex-col gap-4">
          <div class="w-full">
            <h3 class="text-lg text-kiln font-bold mb-2">瓦猫作品</h3>
            <div id="final-cat" class="bg-gray-200 rounded-lg p-2 text-center">
              <img src="https://picsum.photos/180/180?random=13" alt="待装饰瓦猫" class="w-full rounded-lg">
            </div>
          </div>
          <div class="w-full">
            <h3 class="text-lg text-kiln font-bold mb-2">矿物颜料</h3>
            <div class="grid grid-cols-4 gap-1.5 mb-3">
              <div 
                class="pigment-item bg-amber-700 w-7 h-7 rounded-full cursor-pointer" 
                data-color="赭石色"
                @click="selectPigment('赭石色')"
              ></div>
              <div 
                class="pigment-item bg-blue-500 w-7 h-7 rounded-full cursor-pointer" 
                data-color="石青色"
                @click="selectPigment('石青色')"
              ></div>
              <div 
                class="pigment-item bg-red-600 w-7 h-7 rounded-full cursor-pointer" 
                data-color="朱砂红"
                @click="selectPigment('朱砂红')"
              ></div>
              <div 
                class="pigment-item bg-black w-7 h-7 rounded-full cursor-pointer" 
                data-color="墨黑色"
                @click="selectPigment('墨黑色')"
              ></div>
            </div>
            <div class="flex gap-1.5 mb-3">
              <button 
                class="brush-item bg-white border-2 border-gray-300 p-1.5 rounded-lg" 
                data-brush="small"
                @click="selectBrush('small')"
              >
                <i class="fas fa-paint-brush text-xs"></i>
              </button>
              <button 
                class="brush-item bg-white border-2 border-gray-300 p-1.5 rounded-lg" 
                data-brush="medium"
                @click="selectBrush('medium')"
              >
                <i class="fas fa-paint-brush text-sm"></i>
              </button>
              <button 
                class="brush-item bg-white border-2 border-gray-300 p-1.5 rounded-lg" 
                data-brush="large"
                @click="selectBrush('large')"
              >
                <i class="fas fa-paint-brush text-base"></i>
              </button>
            </div>
            <button 
              id="save-work" 
              class="w-full bg-gray-700 text-white px-3 py-1.5 rounded-lg"
              @click="saveWork"
            >
              <i class="fas fa-save mr-1"></i>保存作品
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- 底部对话框提示 -->
    <div id="master-tip" class="fixed bottom-0 left-0 right-0 bg-kiln text-white px-3 py-2 card-shadow z-50">
      <p class="text-sm flex items-center">
        <i class="fas fa-male mr-2"></i>
        <span>{{ tipText }}</span>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';

// 步骤管理
const currentStep = ref(1);
const tipText = ref('工艺大师：请先准备红土和细砂，按传统配比混合哦~');

// 原料制备相关变量
const clayCount = ref(0);
const sandCount = ref(0);
const draggedMaterial = ref('');

// 基础塑形相关变量
const parts = ref([
  { part: '嘴巴', type: 1 },
  { part: '嘴巴', type: 2 },
  { part: '嘴巴', type: 3 },
  { part: '耳朵', type: 1 },
  { part: '耳朵', type: 2 },
  { part: '耳朵', type: 3 },
  { part: '尾巴', type: 1 },
  { part: '尾巴', type: 2 },
  { part: '尾巴', type: 3 },
]);

// 晾晒修整相关变量
const dryingTime = ref(0);
const dryingCompleted = ref(false);
const repairResultText = ref('');
let dryingInterval = null;

// 高温烧制相关变量
const isFiring = ref(false);
const firingCompleted = ref(false);
const selectedGlaze = ref('');

// 装饰赋色相关变量
const selectedPigment = ref('');
const selectedBrush = ref('');

// 切换步骤
const changeStep = (step) => {
  currentStep.value = step;
  updateTipText(step);
  
  // 步骤3进入时启动阴干计时
  if (step === 3 && !dryingCompleted.value) {
    startDrying();
  }
};

// 更新提示文本
const updateTipText = (step) => {
  const tips = {
    1: '工艺大师：请先准备红土和细砂，按传统配比（7:3）混合哦~',
    2: '工艺大师：现在来给瓦猫选择喜欢的部件吧，也可以自由创作哦~',
    3: '工艺大师：耐心等待阴干（2分钟），然后用工具修整出光滑的表面~',
    4: '工艺大师：放入龙窑，让松烟赋予瓦猫古朴的釉色~',
    5: '工艺大师：最后给瓦猫添加点缀，完成你的专属作品吧~'
  };
  tipText.value = tips[step];
};

// 原料拖拽相关方法
const handleDragStart = (e) => {
  draggedMaterial.value = e.target.getAttribute('data-material');
};

const handleDrop = () => {
  if (draggedMaterial.value === '红土' && clayCount.value < 7) {
    clayCount.value++;
  } else if (draggedMaterial.value === '细砂' && sandCount.value < 3) {
    sandCount.value++;
  }
};

const checkMix = () => {
  if (clayCount.value === 7 && sandCount.value === 3) {
    alert('配比正确！可以进入下一步了~');
    changeStep(2);
  }
};

// 基础塑形相关方法
const selectPart = (part, type) => {
  alert(`已选择${part}样式${type}，实际开发中会替换瓦猫对应部位`);
};

const enterFreeDraw = () => {
  alert('进入自由创作模式，实际开发中会打开绘图工具');
};

// 晾晒修整相关方法
const startDrying = () => {
  if (dryingInterval) clearInterval(dryingInterval);
  dryingTime.value = 0;
  dryingInterval = setInterval(() => {
    dryingTime.value++;
    if (dryingTime.value >= 120) { // 2分钟阴干完成
      clearInterval(dryingInterval);
      dryingCompleted.value = true;
    }
  }, 1000);
};

const dryingTimeText = computed(() => {
  if (dryingCompleted.value) return '阴干完成';
  const minutes = Math.floor(dryingTime.value / 60);
  const seconds = dryingTime.value % 60;
  return `阴干中：${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
});

const useTool = (tool) => {
  if (dryingCompleted.value) {
    repairResultText.value = `使用${tool}修整后，瓦猫表面变得光滑细腻~`;
  }
};

// 高温烧制相关方法
const startFiring = () => {
  isFiring.value = true;
  setTimeout(() => {
    isFiring.value = false;
    firingCompleted.value = true;
  }, 3000); // 3秒后烧制完成
};

const selectGlaze = (color) => {
  const colorMap = {
    amber: '琥珀色',
    gray: '青灰色',
    emerald: '松绿色'
  };
  selectedGlaze.value = color;
  alert(`已选择${colorMap[color]}釉色，瓦猫烧制完成！`);
  changeStep(5);
};

// 装饰赋色相关方法
const selectPigment = (color) => {
  selectedPigment.value = color;
  alert(`已选择${color}颜料，实际开发中可以在瓦猫上绘制`);
};

const selectBrush = (size) => {
  const sizeMap = {
    small: '小',
    medium: '中',
    large: '大'
  };
  selectedBrush.value = size;
  alert(`已选择${sizeMap[size]}号画笔，实际开发中可以调整画笔大小`);
};

const saveWork = () => {
  alert('作品保存成功！可以去文创市集兑换实物啦~');
};

// 组件挂载时初始化
onMounted(() => {
  // 初始步骤为1，启动提示
  updateTipText(1);
});

// 组件卸载时清理定时器
onUnmounted(() => {
  if (dryingInterval) clearInterval(dryingInterval);
});
</script>

<style scoped>
/* 主题颜色定义 */
:root {
  --color-clay: #D97652;
  --color-sand: #E6C893;
  --color-kiln: #5D4037;
  --color-glaze: #8D6E63;
  --color-gray: #374151;
}

/* 工具类 */
.smooth-transition {
  transition: all 0.3s ease-in-out;
}

.card-shadow {
  box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
}

/* 流程节点样式 */
.process-node {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: all 0.3s ease-in-out;
  margin-bottom: 2rem;
}

.node-dot {
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 50%;
  background-color: #e5e7eb;
  transition: all 0.3s ease-in-out;
}

.node-line {
  position: absolute;
  left: 50%;
  top: 1.5rem;
  bottom: -2rem;
  width: 0.25rem;
  background-color: #e5e7eb;
  transform: translateX(-50%);
  z-index: 0;
}

/* 交互元素样式 */
.material-item {
  cursor: grab;
  transition: all 0.3s ease-in-out;
}

.material-item:active {
  cursor: grabbing;
  transform: scale(0.95);
}

.part-item {
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.part-item:hover {
  transform: scale(1.05);
  ring: 2px solid var(--color-clay);
}
</style>