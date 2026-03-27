<script setup lang="ts">
import { ref, onMounted } from 'vue'

const currentPhase = ref(0)

// Words pool for the text well
const aiWords = [
  'LLM', 'Transformers', 'Diffusion Models', 'RAG', 'Vector Database',
  'Fine-Tuning', 'Zero-Shot', 'Few-Shot', 'Prompt Engineering',
  'Reinforcement Learning', 'Neural Networks', 'Generative Adversarial Nets',
  'Backpropagation', 'Gradient Descent', 'Latent Space', 'Parameters', 'Tokenization',
  'Embedding', 'Self-Attention', 'Semantic Search', 'Weights & Biases', 'Epoch',
  'Activation Function', 'Chain of Thought', 'Tree of Thoughts', 'LoRA', 'QLoRA',
  'Quantization', 'Flash Attention', 'Vision Transformer', 'ViT', 'MoE',
  'Mixture of Experts', 'GNN', 'Autoencoder', 'CNN', 'RNN', 'LSTM',
  'Adam Optimizer', 'RMSProp', 'Dropout', 'Batch Norm', 'Layer Norm',
  'Overfitting', 'Generalization', 'Distillation', 'BLEU Score', 'ROUGE',
  'Perplexity', 'F1-Score', 'RLHF', 'Alignment', 'Bias Mitigation',
  'Hallucination', 'Explainability', 'Red Teaming', 'CLIP', 'Stable Diffusion',
  'Midjourney', 'DALL-E', 'Sora', 'Data Augmentation', 'Feature Engineering',
  'Pre-training', 'Transfer Learning', 'Tokenization', 'Multi-Head Attention',
  'Positional Encoding', 'Cross-Attention', 'Beam Search', 'Greedy Decoding'
]

// Generate a word particles field
const wordParticles = Array.from({ length: 120 }, (_, i) => {
  const angle = Math.random() * Math.PI * 2
  const distance = 25 + Math.random() * 55
  return {
    id: i,
    text: aiWords[Math.floor(Math.random() * aiWords.length)],
    tx: `${Math.cos(angle) * distance}vw`,
    ty: `${Math.sin(angle) * distance}vh`,
    delay: `${Math.random() * 25}s`,
    duration: `${12 + Math.random() * 18}s`,
    fontSize: `${0.8 + Math.random() * 0.7}rem`,
    opacity: 0.5 + Math.random() * 0.5
  }
})

onMounted(() => {
  // Phase 1: Sun Ray (3s)
  setTimeout(() => { currentPhase.value = 1 }, 3000)

  // Phase 2: Text Well (6s)
  setTimeout(() => { currentPhase.value = 2 }, 6000)
})
</script>

<template>
  <div class="fixed inset-0 bg-black flex items-center justify-center overflow-hidden">
    <!-- Phase 2: Text Well (Infinite streaming AI terms) -->
    <Transition name="fade">
      <div v-if="currentPhase >= 2" class="absolute inset-0 pointer-events-none z-10 flex items-center justify-center">
        <div v-for="particle in wordParticles" :key="particle.id"
             class="absolute font-sans font-medium text-slate-100/90 tracking-widest whitespace-nowrap animate-stream"
             :style="{
               '--tx': particle.tx,
               '--ty': particle.ty,
               fontSize: particle.fontSize,
               opacity: particle.opacity,
               'animation-delay': particle.delay,
               'animation-duration': particle.duration
             }">
          {{ particle.text }}
        </div>
      </div>
    </Transition>

    <!-- Phase 1: Sun Rays -->
    <Transition name="fade-slow">
      <div v-if="currentPhase >= 1" class="absolute inset-0 flex items-center justify-center pointer-events-none">
        <div class="relative w-full h-full flex items-center justify-center">
          <div class="absolute w-[600px] h-[600px] bg-white rounded-full opacity-10 blur-[100px] animate-ray-expand"></div>
          <div class="absolute w-32 h-32 bg-white blur-[40px] rounded-full opacity-60 animate-pulse-slow"></div>
        </div>
      </div>
    </Transition>

    <!-- Phase 0: The Void -->
    <Transition name="fade">
      <div v-if="currentPhase === 0" class="w-full h-full bg-black"></div>
    </Transition>
  </div>
</template>

<style scoped>
@keyframes stream {
  0% { transform: translate(-50%, -50%) scale(0); opacity: 0; }
  12% { opacity: 1; transform: translate(calc(var(--tx) * 0.12 - 50%), calc(var(--ty) * 0.12 - 50%)) scale(0.7); }
  85% { opacity: 1; }
  100% { transform: translate(calc(var(--tx) - 50%), calc(var(--ty) - 50%)) scale(1.1); opacity: 0; }
}

.animate-stream {
  animation-name: stream;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
  animation-fill-mode: both;
}

@keyframes ray-expand {
  0% { transform: scale(0); opacity: 0; }
  100% { transform: scale(1); opacity: 0.2; }
}

@keyframes pulse-slow {
  0%, 100% { opacity: 0.6; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.1); }
}

.animate-ray-expand { 
  animation: ray-expand 4s cubic-bezier(0.2, 0.8, 0.2, 1) forwards; 
}

.animate-pulse-slow { 
  animation: pulse-slow 6s ease-in-out infinite; 
}

.fade-slow-enter-active { transition: opacity 4s ease-in-out; }
.fade-slow-enter-from { opacity: 0; }
.fade-leave-active { transition: opacity 1s ease; }
.fade-leave-to { opacity: 0; }
</style>
