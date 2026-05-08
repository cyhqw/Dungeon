<template>
  <DungeonModal
    title="词条与状态"
    :is-open="isOpen"
    panel-class="glossary-reference-modal !max-w-[min(94vw,980px)]"
    @close="$emit('close')"
  >
    <div class="glossary-reference-root">
      <div class="glossary-reference-tabs" role="tablist" aria-label="帮助分类">
        <button
          v-for="tab in tabs"
          :key="tab.id"
          type="button"
          class="glossary-reference-tab"
          :class="{ active: activeTab === tab.id }"
          @click="activeTab = tab.id"
        >
          {{ tab.label }}
        </button>
      </div>

      <section v-if="activeTab === 'tutorial'" class="glossary-reference-section">
        <div class="glossary-reference-head">
          <div>
            <div class="glossary-reference-title">战斗教程</div>
            <div class="glossary-reference-subtitle">用界面示意快速理解一场战斗怎么跑起来</div>
          </div>
          <span class="glossary-reference-count">5类</span>
        </div>

        <div class="battle-tutorial-nav">
          <span v-for="section in tutorialSections" :key="section">{{ section }}</span>
        </div>

        <div class="battle-tutorial-hero">
          <div class="battle-tutorial-board">
            <div class="battle-tutorial-enemy">
              <div class="tutorial-status-panel tutorial-status-panel--enemy">
                <div class="tutorial-status-name">敌方</div>
                <div class="tutorial-meter-row">
                  <span>HP</span>
                  <div class="tutorial-meter"><i style="width: 72%"></i></div>
                  <strong>42/58</strong>
                </div>
                <div class="tutorial-meter-row">
                  <span>MP</span>
                  <div class="tutorial-meter tutorial-meter--mp"><i style="width: 45%"></i></div>
                  <strong>4</strong>
                </div>
                <div class="tutorial-stat-line"><span>骰子</span><strong>2 ~ 8</strong></div>
                <div class="tutorial-status-icons">
                  <span class="tutorial-effect-chip tutorial-effect-chip--small">护甲 6</span>
                  <span class="tutorial-effect-chip tutorial-effect-chip--small">寒冷 3</span>
                </div>
              </div>
              <div class="battle-tutorial-intent-card">
                <div class="mock-card-cost">3</div>
                <div class="mock-card-name">敌方意图</div>
                <div class="mock-card-line"></div>
                <div class="mock-card-desc">下回合准备造成伤害</div>
              </div>
            </div>
            <div class="battle-tutorial-center">
              <div class="mock-dice mock-dice--red"><span>6</span></div>
              <div class="mock-versus">VS</div>
              <div class="mock-dice mock-dice--blue"><span>4</span></div>
            </div>
            <div class="battle-tutorial-player">
              <div class="tutorial-status-panel tutorial-status-panel--player">
                <div class="tutorial-status-name">我方</div>
                <div class="tutorial-meter-row">
                  <span>HP</span>
                  <div class="tutorial-meter"><i style="width: 83%"></i></div>
                  <strong>34/41</strong>
                </div>
                <div class="tutorial-meter-row">
                  <span>MP</span>
                  <div class="tutorial-meter tutorial-meter--mp"><i style="width: 55%"></i></div>
                  <strong>5</strong>
                </div>
                <div class="tutorial-stat-line"><span>骰子</span><strong>1 ~ 8</strong></div>
              </div>
              <div class="battle-tutorial-hand">
                <div class="mock-mini-card mock-mini-card--attack">攻击</div>
                <div class="mock-mini-card mock-mini-card--magic">魔法</div>
                <div class="mock-mini-card mock-mini-card--skill">功能</div>
                <div class="mock-mini-card mock-mini-card--dodge">闪避</div>
              </div>
              <div class="tutorial-active-slot">
                <div class="tutorial-active-cost">2</div>
                <div class="tutorial-active-badge">主动</div>
                <div class="tutorial-active-name">重掷</div>
                <div class="tutorial-active-cd">CD 2 · 可用</div>
              </div>
            </div>
          </div>
          <div class="battle-tutorial-hero-copy">
            <div class="battle-tutorial-hero-title">战斗界面先读三块。</div>
            <p>上方看回合、疲劳与点数预览；敌人旁看意图和敌方状态栏；下方看手牌、主动技能、自己的生命/魔力/骰子范围。</p>
            <p>每回合的核心问题是：敌人接下来要做什么？我这张牌的最终点数是多少？结算后我能不能活到下回合？</p>
          </div>
        </div>

        <div class="battle-tutorial-grid">
          <article class="battle-tutorial-card battle-tutorial-card--wide">
            <div class="battle-tutorial-card-visual card-type-board">
              <div
                v-for="cardType in cardTypeGuides"
                :key="cardType.name"
                class="tutorial-type-card"
                :class="`tutorial-type-card--${cardType.tone}`"
              >
                <div class="tutorial-type-card-cost">{{ cardType.cost }}</div>
                <div class="tutorial-type-card-art">
                  <i :class="cardType.icon"></i>
                </div>
                <div class="tutorial-type-card-name">{{ cardType.name }}</div>
                <div class="tutorial-type-card-desc">{{ cardType.short }}</div>
              </div>
            </div>
            <div class="battle-tutorial-card-text">
              <h3>卡牌分为六类</h3>
              <p>物理偏直接攻击；魔法常消耗 MP、倍率更高或有额外机制；功能牌通常叠甲、回蓝、抽牌、铺状态；闪避牌专门应对攻击；主动牌来自主动技能槽；诅咒多为负面或污染牌库。</p>
            </div>
          </article>

          <article class="battle-tutorial-card battle-tutorial-card--wide">
            <div class="battle-tutorial-card-visual point-flow">
              <div class="mock-dice mock-dice--blue"><span>5</span></div>
              <div class="flow-arrow">→</div>
              <div class="flow-chip">卡牌 +2</div>
              <div class="flow-arrow">→</div>
              <div class="flow-chip flow-chip--gold">最终 7</div>
              <div class="flow-arrow">→</div>
              <div class="flow-chip flow-chip--red">伤害</div>
            </div>
            <div class="battle-tutorial-card-text">
              <h3>骰子不是最终结果</h3>
              <p>骰子先给基础点数，卡牌倍率/加值、状态、圣遗物和卡牌专属效果会继续修正。界面上方的预览会列出这条计算链，最后向下取整。</p>
            </div>
          </article>

          <article class="battle-tutorial-card battle-tutorial-card--wide">
            <div class="battle-tutorial-card-visual clash-board">
              <div class="clash-side">
                <div class="tutorial-type-card tutorial-type-card--physical">
                  <div class="tutorial-type-card-art"><i class="fa-solid fa-khanda"></i></div>
                  <div class="tutorial-type-card-name">物理</div>
                </div>
                <div class="clash-score">最终 9</div>
              </div>
              <div class="clash-rules">
                <strong>拼点</strong>
                <span>同类攻击比大小</span>
                <span>闪避按特殊判定</span>
                <span>平局双方失败</span>
              </div>
              <div class="clash-side">
                <div class="tutorial-type-card tutorial-type-card--physical">
                  <div class="tutorial-type-card-art"><i class="fa-solid fa-khanda"></i></div>
                  <div class="tutorial-type-card-name">物理</div>
                </div>
                <div class="clash-score clash-score--losing">最终 6</div>
              </div>
            </div>
            <div class="battle-tutorial-card-text">
              <h3>拼点什么时候发生</h3>
              <p>物理对物理、魔法对魔法会拼点，最终点数高的一方成功，低的一方失败；平局时双方都失败。功能牌通常不拼点，会按结算优先级直接执行。</p>
              <p>闪避遇到物理/魔法时也会拼点，但判定很特别：攻击方最终点数高于闪避点数时，闪避成功，攻击失效；否则闪避失败，攻击继续生效。</p>
            </div>
          </article>

          <article class="battle-tutorial-card battle-tutorial-card--wide">
            <div class="battle-tutorial-card-visual modifier-board">
              <span v-for="modifier in clashModifiers" :key="modifier" class="flow-chip">{{ modifier }}</span>
            </div>
            <div class="battle-tutorial-card-text">
              <h3>哪些东西会影响拼点</h3>
              <p>重掷会先改骰子；蓄力、疲劳等会影响原始骰子；卡牌自身的倍率/加值改最终点数；部分状态、圣遗物、主动技能和卡牌专属文字也会继续改点数。</p>
              <p>还有少数效果会绕开拼点，例如“无视闪避”会让攻击跳过闪避拼点；“压势/弱魔术”这类牌会在行动队列前削减对方原始点数。</p>
            </div>
          </article>

          <article class="battle-tutorial-card battle-tutorial-card--wide">
            <div class="battle-tutorial-card-visual tutorial-timeline">
              <div v-for="step in turnSteps" :key="step" class="timeline-step">{{ step }}</div>
            </div>
            <div class="battle-tutorial-card-text">
              <h3>一回合大致这样走</h3>
              <p>回合开始结算状态并抽牌，然后双方掷骰；你选择卡牌后进入拼点与效果结算，最后弃牌并处理回合结束效果。</p>
            </div>
          </article>

          <article class="battle-tutorial-card battle-tutorial-card--wide">
            <div class="battle-tutorial-card-visual active-skill-board">
              <div class="tutorial-active-slot tutorial-active-slot--large">
                <div class="tutorial-active-cost">2</div>
                <div class="tutorial-active-badge">主动</div>
                <div class="tutorial-active-name">抽牌</div>
                <div class="tutorial-active-desc">抽 1 张牌</div>
                <div class="tutorial-active-cd">CD 3 · 1/2</div>
              </div>
              <div class="active-skill-points">
                <span>只在玩家输入阶段可用</span>
                <span>消耗 MP 或次数</span>
                <span>进入冷却</span>
                <span>不占手牌</span>
              </div>
            </div>
            <div class="battle-tutorial-card-text">
              <h3>主动技能是额外工具</h3>
              <p>主动技能显示在战斗左下角，不占手牌格。它们通常用来重掷骰子、抽牌、加护甲、改双方点数或施加特殊效果。</p>
              <p>主动技能只能在玩家输入阶段使用，并会受到 MP、冷却、次数上限、眩晕等限制。用完后通常还能继续选择手牌。</p>
            </div>
          </article>

          <article class="battle-tutorial-card battle-tutorial-card--wide">
            <div class="battle-tutorial-card-visual status-reference-board">
              <div class="tutorial-status-panel tutorial-status-panel--wide">
                <div v-for="metric in statusMetrics" :key="metric.label" class="status-metric-row">
                  <span class="status-metric-label">{{ metric.label }}</span>
                  <strong>{{ metric.value }}</strong>
                  <small>{{ metric.desc }}</small>
                </div>
                <div class="effect-strip">
                  <span v-for="effect in tutorialEffects" :key="effect.type" class="tutorial-effect-chip">
                    <i
                      v-if="effect.faClass"
                      :class="[effect.faClass, 'text-[13px] leading-none']"
                      :style="effect.faStyle"
                      aria-hidden="true"
                    ></i>
                    <span>{{ effect.name }}</span>
                  </span>
                </div>
              </div>
            </div>
            <div class="battle-tutorial-card-text">
              <h3>状态栏要重点看这些</h3>
              <p>HP 是生死线；MP 决定能否使用魔法牌和主动技能；骰子范围决定本回合原始点数的上下限；护甲/中毒量/临时生命上限等会以小图标显示。</p>
              <p>状态图标下方的数字是层数。鼠标或长按查看详细说明；状态的完整解释可以切到“已解锁状态”页。</p>
            </div>
          </article>

          <article class="battle-tutorial-card battle-tutorial-card--wide">
            <div class="battle-tutorial-card-visual choice-map">
              <div class="choice-node choice-node--intent">敌人要攻击</div>
              <div class="choice-branches">
                <span>闪避拼点</span>
                <span>功能牌叠甲</span>
                <span>主动改点</span>
                <span>攻击抢先压血</span>
              </div>
            </div>
            <div class="battle-tutorial-card-text">
              <h3>新手决策顺序</h3>
              <p>先看敌方意图，再看自己的 HP/MP/骰子点数。敌人要攻击时优先保命；敌人要铺垫时可以叠状态或输出；手里有连击牌时，先用它调整资源，再打主牌。</p>
            </div>
          </article>
        </div>
      </section>

      <section v-else-if="activeTab === 'terms'" class="glossary-reference-section">
        <div class="glossary-reference-head">
          <div>
            <div class="glossary-reference-title">卡牌词条</div>
            <div class="glossary-reference-subtitle">卡牌说明中出现的通用规则</div>
          </div>
          <span class="glossary-reference-count">{{ termEntries.length }}</span>
        </div>

        <div class="glossary-reference-grid">
          <article
            v-for="entry in termEntries"
            :key="entry.key"
            class="glossary-reference-card"
            :class="toneClass(entry.polarity)"
          >
            <div class="glossary-reference-card-title">{{ entry.label }}</div>
            <p class="glossary-reference-card-desc">{{ entry.description }}</p>
          </article>
        </div>
      </section>

      <section v-else class="glossary-reference-section">
        <div class="glossary-reference-head">
          <div>
            <div class="glossary-reference-title">已解锁状态</div>
            <div class="glossary-reference-subtitle">来自魔女的收藏中已记录的状态</div>
          </div>
          <span class="glossary-reference-count">{{ unlockedEffectEntries.length }}</span>
        </div>

        <div v-if="unlockedEffectEntries.length > 0" class="glossary-reference-grid">
          <article
            v-for="entry in unlockedEffectEntries"
            :key="entry.type"
            class="glossary-reference-card"
            :class="toneClass(entry.polarity)"
          >
            <div class="glossary-reference-effect-title">
              <span class="glossary-reference-effect-icon">
                <i
                  v-if="entry.faClass"
                  :class="[entry.faClass, 'text-[14px] leading-none']"
                  :style="entry.faStyle"
                  aria-hidden="true"
                ></i>
                <span v-else>{{ entry.name.slice(0, 1) }}</span>
              </span>
              <span>{{ entry.name }}</span>
            </div>
            <p class="glossary-reference-card-desc">{{ entry.description || '暂无说明' }}</p>
          </article>
        </div>
        <div v-else class="glossary-reference-empty">
          尚未记录任何状态。战斗中遇到状态后会自动加入这里。
        </div>
      </section>
    </div>
  </DungeonModal>
</template>

<script setup lang="ts">
import { getCardKeywordGlossaryEntries, getCardTraitGlossaryEntries, type CardGlossaryEntry } from '../battle/cardTextGlossary';
import { EFFECT_REGISTRY, getEffectDisplayOrder } from '../battle/effects';
import { loadCodexState } from '../codexStore';
import { getEffectFontAwesomeClass, getEffectFontAwesomeStyle } from '../effectIconRegistry';
import type { CardTraits, EffectPolarity, EffectType } from '../types';
import DungeonModal from './DungeonModal.vue';

const props = defineProps<{ isOpen: boolean }>();
defineEmits<{ close: [] }>();

type ReferenceTab = 'tutorial' | 'terms' | 'status';

const tabs: Array<{ id: ReferenceTab; label: string }> = [
  { id: 'tutorial', label: '战斗教程' },
  { id: 'terms', label: '卡牌词条' },
  { id: 'status', label: '已解锁状态' },
];
const activeTab = ref<ReferenceTab>('tutorial');
const tutorialSections = ['界面速览', '卡牌类型', '拼点机制', '主动技能', '状态栏'];
const cardTypeGuides = [
  { name: '物理', tone: 'physical', cost: '0', icon: 'fa-solid fa-khanda', short: '直接攻击，常靠点数胜负。' },
  { name: '魔法', tone: 'magic', cost: '2', icon: 'fa-solid fa-wand-sparkles', short: '多消耗 MP，倍率或特效更强。' },
  { name: '功能', tone: 'function', cost: '0', icon: 'fa-solid fa-rotate', short: '叠甲、回蓝、抽牌、铺状态。' },
  { name: '闪避', tone: 'dodge', cost: '0', icon: 'fa-solid fa-shoe-prints', short: '应对物理/魔法攻击。' },
  { name: '主动', tone: 'active', cost: '1', icon: 'fa-solid fa-bolt', short: '来自主动槽，不占手牌。' },
  { name: '诅咒', tone: 'curse', cost: '-', icon: 'fa-solid fa-skull', short: '多为负面、污染或限制牌。' },
];
const clashModifiers = [
  '重掷自己/对手',
  '蓄力 +骰子',
  '疲劳 -骰子',
  '卡牌 x倍率/+加值',
  '闪避受鳞粉加点',
  '主动技能改骰子',
  '圣遗物修正',
  '无视闪避跳过拼点',
];
const statusMetrics = [
  { label: 'HP', value: '34/41', desc: '生命，归零战败。' },
  { label: 'MP', value: '5', desc: '魔力，支付魔法牌和主动技能。' },
  { label: '骰子', value: '1 ~ 8', desc: '本方原始点数的随机范围。' },
  { label: '护甲', value: '6', desc: '抵挡伤害，通常会衰减。' },
  { label: '状态', value: '图标+层数', desc: '影响点数、伤害、行动或回合结算。' },
];

const allTraitEntries = (): CardGlossaryEntry[] => {
  const baseTraits: CardTraits = {
    combo: true,
    reroll: 'self',
    draw: true,
    unplayable: true,
    destroyOnClashWin: true,
    purgeOnUse: true,
    insertCardsToEnemyDeck: ['指定卡牌'],
  };
  return [
    ...getCardTraitGlossaryEntries(baseTraits),
    ...getCardTraitGlossaryEntries({ combo: false, reroll: 'enemy', draw: false }),
  ];
};

const uniqGlossaryEntries = (entries: CardGlossaryEntry[]): CardGlossaryEntry[] => {
  const seen = new Set<string>();
  return entries.filter((entry) => {
    if (seen.has(entry.key)) return false;
    seen.add(entry.key);
    return true;
  });
};

const termEntries = computed(() => uniqGlossaryEntries([
  ...allTraitEntries(),
  ...getCardKeywordGlossaryEntries('负面效果 法力汲取 群攻 逃离 自伤'),
]));

const codex = ref(loadCodexState());
const unlockedEffectTypes = computed(() => new Set(codex.value.effects));
const unlockedEffectEntries = computed(() => (
  Object.entries(EFFECT_REGISTRY)
    .filter(([type]) => unlockedEffectTypes.value.has(type as EffectType))
    .map(([type, def]) => ({
      type,
      name: def.name,
      description: def.description ?? '',
      polarity: def.polarity,
      faClass: getEffectFontAwesomeClass(type as EffectType),
      faStyle: getEffectFontAwesomeStyle(type as EffectType),
    }))
    .sort((a, b) => {
      const orderComp = getEffectDisplayOrder(a.type as EffectType) - getEffectDisplayOrder(b.type as EffectType);
      if (orderComp !== 0) return orderComp;
      return a.name.localeCompare(b.name, 'zh-Hans-CN');
    })
));

const tutorialEffectNames = ['护甲', '寒冷', '燃烧', '中毒', '易伤', '虚弱'];
const tutorialEffects = computed(() => tutorialEffectNames
  .map((name) => {
    const entry = Object.values(EFFECT_REGISTRY).find(effect => effect.name === name);
    if (!entry) return null;
    return {
      type: entry.type,
      name: entry.name,
      faClass: getEffectFontAwesomeClass(entry.type),
      faStyle: getEffectFontAwesomeStyle(entry.type),
    };
  })
  .filter((entry): entry is NonNullable<typeof entry> => Boolean(entry)));
const turnSteps = ['回合开始', '抽牌', '掷骰', '选牌', '结算', '回合结束'];

const toneClass = (polarity?: EffectPolarity | 'trait') => {
  if (polarity === 'buff') return 'glossary-reference-card--buff';
  if (polarity === 'debuff') return 'glossary-reference-card--debuff';
  if (polarity === 'mixed') return 'glossary-reference-card--mixed';
  if (polarity === 'special') return 'glossary-reference-card--special';
  return 'glossary-reference-card--trait';
};

watch(
  () => props.isOpen,
  (open) => {
    if (open) {
      codex.value = loadCodexState();
      activeTab.value = 'tutorial';
    }
  },
  { immediate: true },
);
</script>

<style scoped>
.glossary-reference-root {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.glossary-reference-tabs {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  border: 1px solid rgba(251, 191, 36, 0.18);
  border-radius: 0.5rem;
  background: rgba(0, 0, 0, 0.18);
  padding: 0.4rem;
}

.glossary-reference-tab {
  min-height: 2.1rem;
  border: 1px solid transparent;
  border-radius: 0.42rem;
  padding: 0 0.9rem;
  color: rgba(253, 230, 138, 0.68);
  font-size: 0.78rem;
  transition:
    color 0.18s ease,
    border-color 0.18s ease,
    background 0.18s ease,
    transform 0.18s ease;
}

.glossary-reference-tab:hover {
  color: rgba(254, 243, 199, 0.95);
  border-color: rgba(251, 191, 36, 0.24);
  background: rgba(251, 191, 36, 0.08);
}

.glossary-reference-tab.active {
  border-color: rgba(251, 191, 36, 0.42);
  background: linear-gradient(145deg, rgba(146, 64, 14, 0.42), rgba(69, 26, 3, 0.38));
  color: rgba(255, 247, 237, 0.98);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.08);
}

.glossary-reference-section {
  border: 1px solid rgba(251, 191, 36, 0.2);
  border-radius: 0.5rem;
  background:
    linear-gradient(145deg, rgba(16, 12, 10, 0.86), rgba(35, 24, 15, 0.58)),
    radial-gradient(circle at 12% 0%, rgba(251, 191, 36, 0.14), transparent 40%);
  padding: 1rem;
}

.glossary-reference-head {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 0.75rem;
  margin-bottom: 0.85rem;
}

.glossary-reference-title {
  color: rgba(254, 243, 199, 0.98);
  font-family: var(--font-heading);
  font-size: 1rem;
  letter-spacing: 0.12em;
}

.glossary-reference-subtitle {
  margin-top: 0.12rem;
  color: rgba(253, 230, 138, 0.56);
  font-size: 0.75rem;
}

.glossary-reference-count {
  min-width: 2rem;
  border: 1px solid rgba(251, 191, 36, 0.34);
  border-radius: 9999px;
  background: rgba(0, 0, 0, 0.24);
  color: rgba(253, 230, 138, 0.9);
  font-size: 0.75rem;
  line-height: 1.35rem;
  text-align: center;
}

.glossary-reference-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(13rem, 1fr));
  gap: 0.65rem;
}

.glossary-reference-card {
  min-height: 5.8rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-left-width: 3px;
  border-radius: 0.5rem;
  background: rgba(0, 0, 0, 0.24);
  padding: 0.72rem 0.78rem;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.04);
}

.glossary-reference-card--buff {
  border-left-color: rgba(74, 222, 128, 0.82);
}

.glossary-reference-card--debuff {
  border-left-color: rgba(248, 113, 113, 0.82);
}

.glossary-reference-card--mixed {
  border-left-color: rgba(168, 85, 247, 0.82);
}

.glossary-reference-card--special {
  border-left-color: rgba(56, 189, 248, 0.82);
}

.glossary-reference-card--trait {
  border-left-color: rgba(251, 191, 36, 0.82);
}

.glossary-reference-card-title,
.glossary-reference-effect-title {
  display: flex;
  align-items: center;
  gap: 0.45rem;
  color: rgba(255, 247, 237, 0.95);
  font-size: 0.88rem;
  font-weight: 700;
}

.glossary-reference-effect-icon {
  display: inline-flex;
  width: 1.45rem;
  height: 1.45rem;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(251, 191, 36, 0.22);
  border-radius: 9999px;
  background: rgba(0, 0, 0, 0.26);
  color: rgba(253, 230, 138, 0.9);
  font-size: 0.72rem;
}

.glossary-reference-card-desc {
  margin-top: 0.42rem;
  color: rgba(255, 237, 213, 0.72);
  font-size: 0.76rem;
  line-height: 1.55;
}

.glossary-reference-empty {
  border: 1px dashed rgba(251, 191, 36, 0.26);
  border-radius: 0.5rem;
  padding: 1.6rem;
  color: rgba(253, 230, 138, 0.62);
  text-align: center;
}

.battle-tutorial-nav {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-bottom: 0.85rem;
}

.battle-tutorial-nav span {
  border: 1px solid rgba(251, 191, 36, 0.18);
  border-radius: 9999px;
  background: rgba(0, 0, 0, 0.18);
  color: rgba(253, 230, 138, 0.7);
  padding: 0.18rem 0.58rem;
  font-size: 0.68rem;
}

.battle-tutorial-hero {
  display: grid;
  grid-template-columns: minmax(0, 1.28fr) minmax(14rem, 0.72fr);
  gap: 0.85rem;
  align-items: stretch;
  margin-bottom: 0.85rem;
}

.battle-tutorial-board {
  min-height: 15rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 0.5rem;
  background:
    linear-gradient(180deg, rgba(18, 20, 28, 0.92), rgba(8, 8, 13, 0.92)),
    radial-gradient(circle at 72% 24%, rgba(127, 29, 29, 0.32), transparent 34%),
    radial-gradient(circle at 22% 82%, rgba(30, 64, 175, 0.28), transparent 36%);
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  gap: 0.8rem;
  padding: 1rem;
  overflow: hidden;
}

.battle-tutorial-enemy,
.battle-tutorial-player,
.battle-tutorial-center {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 0.75rem;
}

.tutorial-status-panel {
  width: 11.2rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 0.58rem;
  background: rgba(24, 20, 30, 0.9);
  padding: 0.55rem;
  box-shadow:
    0 10px 24px rgba(0, 0, 0, 0.28),
    inset 0 1px 0 rgba(255, 255, 255, 0.05);
}

.tutorial-status-panel--enemy {
  border-color: rgba(248, 113, 113, 0.2);
}

.tutorial-status-panel--player {
  border-color: rgba(96, 165, 250, 0.2);
}

.tutorial-status-name {
  margin-bottom: 0.42rem;
  color: rgba(255, 247, 237, 0.92);
  font-size: 0.74rem;
  font-weight: 800;
}

.tutorial-meter-row,
.tutorial-stat-line {
  display: grid;
  grid-template-columns: 1.8rem minmax(0, 1fr) 2.8rem;
  gap: 0.35rem;
  align-items: center;
  color: rgba(255, 237, 213, 0.68);
  font-size: 0.62rem;
}

.tutorial-stat-line {
  grid-template-columns: 1.8rem 1fr;
  margin-top: 0.35rem;
}

.tutorial-meter-row + .tutorial-meter-row {
  margin-top: 0.35rem;
}

.tutorial-meter-row strong,
.tutorial-stat-line strong {
  color: rgba(254, 243, 199, 0.95);
  font-size: 0.62rem;
  text-align: right;
}

.tutorial-meter {
  height: 0.45rem;
  border: 1px solid rgba(127, 29, 29, 0.35);
  border-radius: 9999px;
  background: rgba(26, 10, 10, 0.9);
  overflow: hidden;
}

.tutorial-meter i {
  display: block;
  height: 100%;
  border-radius: inherit;
  background: linear-gradient(90deg, rgb(204, 34, 0), rgb(238, 51, 17));
}

.tutorial-meter--mp {
  border-color: rgba(30, 64, 175, 0.35);
  background: rgba(10, 10, 26, 0.9);
}

.tutorial-meter--mp i {
  background: linear-gradient(90deg, rgb(0, 102, 204), rgb(0, 136, 238));
}

.tutorial-status-icons {
  display: flex;
  flex-wrap: wrap;
  gap: 0.3rem;
  margin-top: 0.45rem;
}

.battle-tutorial-enemy {
  align-items: flex-start;
}

.battle-tutorial-player {
  align-items: flex-end;
}

.battle-tutorial-center {
  align-items: center;
}

.battle-tutorial-intent-card,
.tutorial-card-preview {
  width: 8.5rem;
  border: 1px solid rgba(251, 191, 36, 0.32);
  border-radius: 0.5rem;
  background:
    linear-gradient(160deg, rgba(39, 28, 19, 0.96), rgba(15, 12, 16, 0.96)),
    radial-gradient(circle at 50% 0%, rgba(251, 191, 36, 0.14), transparent 46%);
  padding: 0.55rem;
  color: rgba(255, 247, 237, 0.9);
  box-shadow:
    0 10px 24px rgba(0, 0, 0, 0.35),
    inset 0 1px 0 rgba(255, 255, 255, 0.08);
}

.mock-card-cost {
  display: inline-flex;
  width: 1.35rem;
  height: 1.35rem;
  align-items: center;
  justify-content: center;
  border-radius: 9999px;
  background: rgba(127, 29, 29, 0.82);
  color: rgba(254, 226, 226, 0.95);
  font-weight: 700;
}

.mock-card-name,
.tutorial-card-title {
  margin-top: 0.35rem;
  color: rgba(254, 243, 199, 0.98);
  font-size: 0.78rem;
  font-weight: 800;
}

.mock-card-line {
  height: 3.7rem;
  margin: 0.45rem 0;
  border-radius: 0.35rem;
  background:
    linear-gradient(135deg, rgba(239, 68, 68, 0.18), rgba(251, 191, 36, 0.14)),
    repeating-linear-gradient(135deg, rgba(255,255,255,0.08) 0 1px, transparent 1px 7px);
}

.mock-card-desc,
.tutorial-card-desc {
  color: rgba(255, 237, 213, 0.68);
  font-size: 0.66rem;
  line-height: 1.4;
}

.battle-tutorial-status-bar {
  display: inline-flex;
  align-items: center;
  gap: 0.38rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 9999px;
  background: rgba(0, 0, 0, 0.32);
  padding: 0.28rem 0.55rem;
  color: rgba(255, 237, 213, 0.72);
  font-size: 0.68rem;
}

.battle-tutorial-status-bar strong {
  color: rgba(254, 243, 199, 0.98);
}

.status-dot {
  width: 0.55rem;
  height: 0.55rem;
  border-radius: 9999px;
}

.status-dot--hp {
  background: rgb(239, 68, 68);
  box-shadow: 0 0 10px rgba(239, 68, 68, 0.55);
}

.status-dot--mp {
  background: rgb(96, 165, 250);
  box-shadow: 0 0 10px rgba(96, 165, 250, 0.55);
}

.mock-dice {
  display: inline-flex;
  width: 3rem;
  height: 3rem;
  align-items: center;
  justify-content: center;
  border: 2px solid rgba(255, 255, 255, 0.18);
  border-radius: 0.58rem;
  color: white;
  font-size: 1.28rem;
  font-weight: 900;
  transform: rotate(45deg);
  box-shadow: 0 10px 24px rgba(0, 0, 0, 0.38);
}

.mock-dice--red {
  background: linear-gradient(145deg, rgba(127, 29, 29, 0.95), rgba(220, 38, 38, 0.78));
}

.mock-dice--blue {
  background: linear-gradient(145deg, rgba(30, 64, 175, 0.95), rgba(14, 165, 233, 0.72));
}

.mock-dice {
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.55);
}

.mock-dice span {
  line-height: 1;
  transform: rotate(-45deg);
}

.mock-versus {
  color: rgba(253, 230, 138, 0.76);
  font-size: 0.74rem;
  font-weight: 800;
  letter-spacing: 0.12em;
}

.battle-tutorial-hand,
.combo-demo {
  display: flex;
  gap: 0.4rem;
  align-items: center;
}

.mock-mini-card {
  display: inline-flex;
  width: 3.5rem;
  height: 4.6rem;
  align-items: flex-end;
  justify-content: center;
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 0.42rem;
  padding-bottom: 0.42rem;
  color: rgba(255, 247, 237, 0.9);
  font-size: 0.68rem;
  font-weight: 700;
  box-shadow: 0 8px 18px rgba(0, 0, 0, 0.28);
}

.mock-mini-card--attack {
  background: linear-gradient(160deg, rgba(127, 29, 29, 0.86), rgba(38, 18, 18, 0.94));
}

.mock-mini-card--magic {
  background: linear-gradient(160deg, rgba(88, 28, 135, 0.86), rgba(30, 18, 45, 0.94));
}

.mock-mini-card--skill {
  background: linear-gradient(160deg, rgba(133, 77, 14, 0.86), rgba(42, 30, 12, 0.94));
}

.mock-mini-card--dodge {
  background: linear-gradient(160deg, rgba(21, 128, 61, 0.74), rgba(12, 28, 22, 0.94));
}

.battle-tutorial-hero-copy {
  border: 1px solid rgba(251, 191, 36, 0.22);
  border-radius: 0.5rem;
  background: rgba(0, 0, 0, 0.24);
  padding: 1rem;
}

.battle-tutorial-hero-title {
  color: rgba(254, 243, 199, 0.98);
  font-size: 1rem;
  font-weight: 800;
}

.battle-tutorial-hero-copy p {
  margin-top: 0.65rem;
  color: rgba(255, 237, 213, 0.72);
  font-size: 0.78rem;
  line-height: 1.7;
}

.battle-tutorial-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 0.7rem;
}

.battle-tutorial-card {
  display: grid;
  grid-template-columns: minmax(9rem, 0.85fr) minmax(0, 1fr);
  gap: 0.75rem;
  align-items: center;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 0.5rem;
  background: rgba(0, 0, 0, 0.23);
  padding: 0.8rem;
}

.battle-tutorial-card--wide {
  grid-column: 1 / -1;
  grid-template-columns: minmax(16rem, 1.15fr) minmax(0, 0.85fr);
}

.battle-tutorial-card-visual {
  min-height: 6.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(251, 191, 36, 0.14);
  border-radius: 0.42rem;
  background:
    linear-gradient(145deg, rgba(15, 23, 42, 0.5), rgba(0, 0, 0, 0.18)),
    repeating-linear-gradient(90deg, rgba(255,255,255,0.025) 0 1px, transparent 1px 10px);
  padding: 0.7rem;
}

.battle-tutorial-card-text h3 {
  color: rgba(255, 247, 237, 0.94);
  font-size: 0.88rem;
  font-weight: 800;
}

.battle-tutorial-card-text p {
  margin-top: 0.42rem;
  color: rgba(255, 237, 213, 0.68);
  font-size: 0.74rem;
  line-height: 1.6;
}

.point-flow,
.tutorial-timeline,
.effect-strip,
.choice-map {
  gap: 0.45rem;
  flex-wrap: wrap;
}

.flow-arrow,
.plus-sign {
  color: rgba(253, 230, 138, 0.68);
  font-weight: 900;
}

.flow-chip,
.timeline-step,
.choice-node,
.choice-branches span,
.tutorial-effect-chip {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.28rem;
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 9999px;
  background: rgba(0, 0, 0, 0.28);
  color: rgba(255, 237, 213, 0.78);
  padding: 0.28rem 0.55rem;
  font-size: 0.7rem;
  white-space: nowrap;
}

.flow-chip--gold {
  border-color: rgba(251, 191, 36, 0.4);
  color: rgba(254, 243, 199, 0.98);
}

.flow-chip--red {
  border-color: rgba(248, 113, 113, 0.42);
  color: rgba(254, 202, 202, 0.96);
}

.tutorial-card-preview {
  width: 7.6rem;
}

.tutorial-card-top {
  display: flex;
  justify-content: space-between;
  color: rgba(253, 230, 138, 0.72);
  font-size: 0.62rem;
}

.tutorial-card-art {
  display: flex;
  height: 3.7rem;
  align-items: center;
  justify-content: center;
  margin: 0.45rem 0;
  border-radius: 0.35rem;
  background: radial-gradient(circle, rgba(251, 191, 36, 0.22), rgba(59, 130, 246, 0.12), transparent 72%);
  color: rgba(251, 191, 36, 0.95);
  font-size: 1.4rem;
}

.tutorial-effect-chip {
  min-width: 4.5rem;
  border-radius: 0.38rem;
}

.tutorial-timeline {
  justify-content: flex-start;
}

.timeline-step {
  border-radius: 0.38rem;
}

.choice-map {
  flex-direction: column;
}

.choice-node--intent {
  border-color: rgba(248, 113, 113, 0.38);
  color: rgba(254, 202, 202, 0.96);
}

.choice-branches {
  display: flex;
  gap: 0.35rem;
  flex-wrap: wrap;
  justify-content: center;
}

.card-type-board {
  align-items: stretch;
  justify-content: flex-start;
  gap: 0.48rem;
  overflow-x: auto;
}

.tutorial-type-card {
  width: 6.25rem;
  min-height: 8.5rem;
  flex: 0 0 auto;
  position: relative;
  border: 2px solid rgba(255, 255, 255, 0.14);
  border-radius: 0.62rem;
  background: rgba(22, 18, 30, 0.95);
  padding: 0.48rem;
  color: rgba(255, 247, 237, 0.9);
  box-shadow: 0 8px 18px rgba(0, 0, 0, 0.28);
}

.tutorial-type-card--physical {
  border-color: rgba(127, 29, 29, 0.88);
  background: linear-gradient(160deg, rgba(69, 10, 10, 0.72), rgba(22, 18, 30, 0.96));
}

.tutorial-type-card--magic {
  border-color: rgba(88, 28, 135, 0.9);
  background: linear-gradient(160deg, rgba(59, 7, 100, 0.66), rgba(22, 18, 30, 0.96));
}

.tutorial-type-card--function {
  border-color: rgba(133, 77, 14, 0.92);
  background: linear-gradient(160deg, rgba(113, 63, 18, 0.62), rgba(22, 18, 30, 0.96));
}

.tutorial-type-card--dodge {
  border-color: rgba(6, 78, 59, 0.9);
  background: linear-gradient(160deg, rgba(6, 78, 59, 0.62), rgba(22, 18, 30, 0.96));
}

.tutorial-type-card--active {
  border-color: rgba(228, 228, 231, 0.72);
  background: linear-gradient(160deg, rgba(212, 212, 216, 0.16), rgba(22, 18, 30, 0.96));
}

.tutorial-type-card--curse {
  border-color: rgba(0, 0, 0, 0.95);
  background: linear-gradient(160deg, rgba(0, 0, 0, 0.78), rgba(22, 18, 30, 0.96));
}

.tutorial-type-card-cost {
  display: inline-flex;
  width: 1.35rem;
  height: 1.35rem;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(192, 132, 252, 0.38);
  border-radius: 9999px;
  background: rgba(126, 34, 206, 0.82);
  color: white;
  font-size: 0.66rem;
  font-weight: 800;
}

.tutorial-type-card-art {
  display: flex;
  height: 3rem;
  align-items: center;
  justify-content: center;
  margin: 0.42rem 0;
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 0.4rem;
  background: rgba(0, 0, 0, 0.28);
  color: rgba(254, 243, 199, 0.82);
  font-size: 1.1rem;
}

.tutorial-type-card-name {
  color: rgba(254, 243, 199, 0.98);
  font-size: 0.74rem;
  font-weight: 900;
  text-align: center;
}

.tutorial-type-card-desc {
  margin-top: 0.3rem;
  color: rgba(255, 237, 213, 0.66);
  font-size: 0.62rem;
  line-height: 1.38;
  text-align: center;
}

.clash-board {
  display: grid;
  grid-template-columns: 1fr minmax(8rem, 0.9fr) 1fr;
  gap: 0.65rem;
}

.clash-side,
.clash-rules,
.active-skill-points {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.42rem;
}

.clash-side .tutorial-type-card {
  min-height: 6.6rem;
}

.clash-score {
  border: 1px solid rgba(74, 222, 128, 0.36);
  border-radius: 9999px;
  background: rgba(20, 83, 45, 0.22);
  color: rgba(187, 247, 208, 0.96);
  padding: 0.2rem 0.52rem;
  font-size: 0.72rem;
  font-weight: 800;
}

.clash-score--losing {
  border-color: rgba(248, 113, 113, 0.34);
  background: rgba(127, 29, 29, 0.2);
  color: rgba(254, 202, 202, 0.96);
}

.clash-rules {
  border: 1px solid rgba(251, 191, 36, 0.22);
  border-radius: 0.48rem;
  background: rgba(0, 0, 0, 0.2);
  padding: 0.7rem;
}

.clash-rules strong {
  color: rgba(254, 243, 199, 0.96);
}

.clash-rules span,
.active-skill-points span {
  color: rgba(255, 237, 213, 0.72);
  font-size: 0.68rem;
}

.modifier-board,
.active-skill-board,
.status-reference-board {
  gap: 0.5rem;
  flex-wrap: wrap;
  justify-content: flex-start;
}

.tutorial-active-slot {
  width: 7.5rem;
  min-height: 4.9rem;
  position: relative;
  border: 2px solid rgba(244, 244, 245, 0.76);
  border-radius: 0.62rem;
  background:
    radial-gradient(circle at 32% 18%, rgba(255, 255, 255, 0.2), rgba(20, 18, 28, 0.95) 66%);
  padding: 0.5rem;
  color: rgba(255, 247, 237, 0.9);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.28);
}

.tutorial-active-slot--large {
  width: 8.8rem;
  min-height: 8.8rem;
}

.tutorial-active-cost {
  display: inline-flex;
  width: 1.45rem;
  height: 1.45rem;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(192, 132, 252, 0.4);
  border-radius: 9999px;
  background: rgba(126, 34, 206, 0.8);
  color: white;
  font-size: 0.68rem;
  font-weight: 800;
}

.tutorial-active-badge {
  position: absolute;
  right: 0.45rem;
  top: 0.45rem;
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 9999px;
  background: rgba(0, 0, 0, 0.55);
  padding: 0.1rem 0.35rem;
  color: rgba(244, 244, 245, 0.9);
  font-size: 0.58rem;
}

.tutorial-active-name {
  margin-top: 0.5rem;
  color: rgba(255, 247, 237, 0.96);
  font-size: 0.78rem;
  font-weight: 900;
  text-align: center;
}

.tutorial-active-desc {
  margin-top: 0.65rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 0.35rem;
  background: rgba(0, 0, 0, 0.35);
  padding: 0.42rem;
  color: rgba(255, 237, 213, 0.68);
  font-size: 0.66rem;
  text-align: center;
}

.tutorial-active-cd {
  margin-top: 0.5rem;
  color: rgba(187, 247, 208, 0.86);
  font-size: 0.62rem;
  text-align: center;
}

.tutorial-status-panel--wide {
  width: min(100%, 28rem);
}

.status-metric-row {
  display: grid;
  grid-template-columns: 3.2rem 4.6rem minmax(0, 1fr);
  gap: 0.5rem;
  align-items: center;
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
  padding: 0.35rem 0;
}

.status-metric-label {
  color: rgba(253, 230, 138, 0.8);
  font-size: 0.68rem;
  font-weight: 900;
}

.status-metric-row strong {
  color: rgba(255, 247, 237, 0.95);
  font-size: 0.7rem;
}

.status-metric-row small {
  color: rgba(255, 237, 213, 0.65);
  font-size: 0.64rem;
  line-height: 1.35;
}

.tutorial-effect-chip--small {
  min-width: 0;
  padding: 0.16rem 0.38rem;
  font-size: 0.6rem;
}

@media (max-width: 760px) {
  .battle-tutorial-hero,
  .battle-tutorial-grid,
  .battle-tutorial-card,
  .battle-tutorial-card--wide {
    grid-template-columns: 1fr;
  }

  .battle-tutorial-card--wide {
    grid-column: auto;
  }

  .battle-tutorial-board {
    grid-template-columns: 1fr;
  }

  .battle-tutorial-enemy,
  .battle-tutorial-player {
    align-items: center;
  }

  .clash-board {
    grid-template-columns: 1fr;
  }

  .status-metric-row {
    grid-template-columns: 2.7rem 3.8rem minmax(0, 1fr);
  }
}
</style>
