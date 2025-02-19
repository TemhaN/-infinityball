<template>
	<!-- Кнопка бургера -->
	<button class="burger-btn" @click="toggleMenu">
		<input type="checkbox" id="check" />
		<span class="line-1"></span>
		<span class="line-2"></span>
		<span class="line-3"></span>
	</button>

	<div class="menu" :class="{ menu: true, show: isMenuOpen }">
		<div class="background-menu-close" @click="toggleMenu"></div>
		<div class="settings">
			<h2 class="text-settings">Настройки игры</h2>
			<div class="section-rings section">
				<h1 class="sec-text">Кольца</h1>
				<!-- Кнопка для раскрытия первой вкладки (Физические свойства) -->
				<div class="section-header" @click="toggletabs('physical')">
					<span>Физические свойства</span>
					<i
						class="caret-icon"
						:class="{ rotated: activeTab === 'physical' }"
					></i>
				</div>

				<transition
					name="slide"
					@before-enter="beforeEnter"
					@enter="enter"
					@leave="leave"
				>
					<div
						v-if="activeTab === 'physical'"
						ref="settingsMenu"
						class="settings-menu"
					>
						<div class="setting-item">
							<p for="numRings">Количество колец:</p>
							<input
								class="range"
								type="range"
								id="numRings"
								v-model="gameSettings.numRings"
								min="1"
								max="200"
							/>
							<span>{{ gameSettings.numRings }}</span>
						</div>
						<div class="setting-item">
							<p for="ringGap">Расстояние между кольцами:</p>
							<input
								type="range"
								id="ringGap"
								v-model="gameSettings.ringGap"
								min="1"
								max="100"
							/>
							<span>{{ gameSettings.ringGap }}</span>
						</div>
						<div class="setting-item">
							<p for="ringThickness">Толщина колец:</p>
							<input
								type="range"
								id="ringThickness"
								v-model="gameSettings.ringThickness"
								min="1"
								max="50"
							/>
							<span>{{ gameSettings.ringThickness }}</span>
						</div>
						<div class="setting-item">
							<p for="ringRotationSpeed">Скорость колец:</p>
							<input
								type="range"
								id="ringRotationSpeed"
								v-model="gameSettings.ringRotationSpeed"
								min="0"
								max="100"
							/>
							<span>{{ gameSettings.ringRotationSpeed }}</span>
						</div>

						<div class="setting-item">
							<p for="gapWidth">Ширина щели:</p>
							<input
								type="range"
								id="gapWidth"
								v-model="gameSettings.gapWidth"
								min="0"
								max="100"
							/>
							<span>{{ gameSettings.gapWidth }}</span>
						</div>
						<div class="setting-menu-dop">
							<p>Плюшки</p>
							<div class="setting-item">
								<button @click="toggleSetting('shrinkingRing')">
									Уменьшать текущее кольцо: {{ gameSettings.shrinkingRing }}
								</button>
							</div>
							<div class="setting-item">
								<p for="shrinkingSpeed">Скорость уменьшения кольца:</p>
								<input
									type="range"
									id="shrinkingSpeed"
									v-model="gameSettings.shrinkingSpeed"
									min="1"
									max="200"
								/>
								<span>{{ gameSettings.shrinkingSpeed }}</span>
							</div>
							<div class="setting-item">
								<p for="minRingRadius">Минимальный радиус уменьшения кольца:</p>
								<input
									type="range"
									id="minRingRadius"
									v-model="gameSettings.minRingRadius"
									min="1"
									max="300"
								/>
								<span>{{ gameSettings.minRingRadius }}</span>
							</div>
							<div class="setting-item">
								<button @click="toggleSetting('moveOnlyCurrentRing')">
									Двигать только текущее кольцо:
									{{ gameSettings.moveOnlyCurrentRing }}
								</button>
							</div>
							<p class="TEST">TEST</p>
							<div class="setting-item">
								<button @click="toggleSetting('ringOffsetX')">
									Смещение колец по оси X: {{ gameSettings.ringOffsetX }}
								</button>
							</div>
							<div class="setting-item">
								<button @click="toggleSetting('ringOffsetY')">
									Смещение колец по оси Y: {{ gameSettings.ringOffsetY }}
								</button>
							</div>
						</div>
					</div>
				</transition>
				<div class="section-header" @click="toggletabs('visual')">
					<span>Физуальные настройки</span>
					<i
						class="caret-icon"
						:class="{ rotated: activeTab === 'visual' }"
					></i>
				</div>
				<transition
					name="slide"
					@before-enter="beforeEnter"
					@enter="enter"
					@leave="leave"
				>
					<div
						v-if="activeTab === 'visual'"
						ref="settingsMenu"
						class="settings-menu"
					>
						<!-- Для выбора типа градиента -->
						<div class="setting-item">
							<p for="gradientType">Тип градиента:</p>
							<div class="tabs">
								<input
									type="radio"
									id="linear"
									name="gradientType"
									value="linear"
									v-model="gameSettings.gradientType"
								/>
								<label class="tab" for="linear">Линейный</label>

								<input
									type="radio"
									id="radial"
									name="gradientType"
									value="radial"
									v-model="gameSettings.gradientType"
								/>
								<label class="tab" for="radial">Радиальный</label>

								<input
									type="radio"
									id="conic"
									name="gradientType"
									value="conic"
									v-model="gameSettings.gradientType"
								/>
								<label class="tab" for="conic">Конический</label>

								<span class="glider"></span>
							</div>
						</div>

						<!-- Для выбора режима цвета колец -->
						<div class="setting-item">
							<p for="ringColorMode">Режим цвета колец:</p>
							<div class="tabs">
								<input
									type="radio"
									id="static"
									name="ringColorMode"
									value="static"
									v-model="gameSettings.ringColorMode"
								/>
								<label class="tab" for="static">Статический</label>

								<input
									type="radio"
									id="random"
									name="ringColorMode"
									value="random"
									v-model="gameSettings.ringColorMode"
								/>
								<label class="tab" for="random">Случайный</label>

								<input
									type="radio"
									id="gradient"
									name="ringColorMode"
									value="gradient"
									v-model="gameSettings.ringColorMode"
								/>
								<label class="tab" for="gradient">Градиент</label>

								<span class="glider"></span>
							</div>
						</div>
						<!-- Статический цвет колец -->
						<div class="setting-item">
							<p for="staticRingColor">Статический цвет колец:</p>
							<input
								type="color"
								id="staticRingColor"
								v-model="gameSettings.staticRingColor"
							/>
						</div>

						<!-- Начальный радиус градиента -->
						<div class="setting-item">
							<p for="gradientStartRadius">Начальный радиус градиента:</p>
							<input
								class="number"
								type="number"
								id="gradientStartRadius"
								v-model="gameSettings.gradientStartRadius"
								min="0"
								max="100"
							/>
						</div>

						<!-- Конечный радиус градиента -->
						<div class="setting-item">
							<p for="gradientEndRadius">Конечный радиус градиента:</p>
							<input
								class="number"
								type="number"
								id="gradientEndRadius"
								v-model="gameSettings.gradientEndRadius"
								min="0"
								max="100"
							/>
						</div>

						<!-- Центр градиента (X) -->
						<div class="setting-item">
							<p for="gradientCenterX">Центр градиента (X):</p>
							<input
								class="number"
								type="number"
								id="gradientCenterX"
								v-model="gameSettings.gradientCenterX"
							/>
						</div>

						<!-- Центр градиента (Y) -->
						<div class="setting-item">
							<p for="gradientCenterY">Центр градиента (Y):</p>
							<input
								class="number"
								type="number"
								id="gradientCenterY"
								v-model="gameSettings.gradientCenterY"
							/>
						</div>

						<!-- Начальный цвет градиента -->
						<div class="setting-item">
							<p for="gradientStartColor">Начальный цвет градиента колец:</p>
							<input
								type="color"
								id="gradientStartColor"
								v-model="gameSettings.gradientStartColor"
							/>
						</div>

						<!-- Конечный цвет градиента -->
						<div class="setting-item">
							<p for="gradientEndColor">Конечный цвет градиента колец:</p>
							<input
								type="color"
								id="gradientEndColor"
								v-model="gameSettings.gradientEndColor"
							/>
						</div>

						<div class="setting-item">
							<button @click="toggleSetting('ringGlow')">
								Свечение колец: {{ gameSettings.ringGlow }}
							</button>
						</div>
						<div class="setting-item">
							<p for="ringGlowColor">Цвет свечения колец:</p>
							<input
								type="color"
								id="ringGlowColor"
								v-model="gameSettings.ringGlowColor"
							/>
						</div>
						<div class="setting-item">
							<p for="ringGlowBlur">Радиус размытия свечения колец:</p>
							<input
								type="range"
								id="ringGlowBlur"
								v-model="gameSettings.ringGlowBlur"
								min="1"
								max="30"
							/>
							<span>{{ gameSettings.ringGlowBlur }}</span>
						</div>
					</div>
				</transition>
			</div>
			<div class="section-ball section">
				<h1 class="sec-text">Мячик</h1>
				<div class="section-header" @click="toggletabs('physical-ball')">
					<span>Физические свойства</span>
					<i
						class="caret-icon"
						:class="{ rotated: activeTab === 'physical-ball' }"
					></i>
				</div>
				<transition
					name="slide"
					@before-enter="beforeEnter"
					@enter="enter"
					@leave="leave"
				>
					<div
						v-if="activeTab === 'physical-ball'"
						ref="settingsMenu"
						class="settings-menu"
					>
						<div class="setting-item">
							<p for="ballRadius">Радиус мяча:</p>
							<input
								type="range"
								id="ballRadius"
								v-model="gameSettings.ballRadius"
								min="0"
								max="100"
							/>
							<span>{{ gameSettings.ballRadius }}</span>
						</div>
						<div class="setting-item">
							<p for="gravity">Гравитация:</p>
							<input
								type="range"
								id="gravity"
								v-model="gameSettings.gravity"
								min="0"
								max="100"
							/>
							<span>{{ gameSettings.gravity }}</span>
						</div>
						<div class="setting-item">
							<p for="bounceSpeed">Сила отскока мяча:</p>
							<input
								type="range"
								id="bounceSpeed"
								v-model="gameSettings.bounceSpeed"
								min="0"
								max="10"
							/>
							<span>{{ gameSettings.bounceSpeed }}</span>
						</div>
						<div class="setting-menu-dop">
							<p>
								Плюшки<span style="color: red">
									TEST TEST НЕ ВКЛЮЧАТЬ ВЫЗЫВАЕТ БАГИ</span
								>
							</p>
							<div class="setting-item">
								<button @click="toggleSetting('ballRadiusIncreaseEnabled')">
									Увеличение радиуса мяча:
									{{ gameSettings.ballRadiusIncreaseEnabled }}
								</button>
							</div>
							<div class="setting-item">
								<button @click="toggleSetting('ballRadiusIncreaseRate')">
									Скорость увеличения радиуса мяча:
									{{ gameSettings.ballRadiusIncreaseRate }}
								</button>
							</div>
						</div>
					</div>
				</transition>
				<div class="section-header" @click="toggletabs('visual-ball')">
					<span>Физуальные настройки</span>
					<i
						class="caret-icon"
						:class="{ rotated: activeTab === 'visual-ball' }"
					></i>
				</div>
				<transition
					name="slide"
					@before-enter="beforeEnter"
					@enter="enter"
					@leave="leave"
				>
					<div
						v-if="activeTab === 'visual-ball'"
						ref="settingsMenu"
						class="settings-menu"
					>
						<div class="setting-item">
							<p for="ballColorMode">Режим цвета мяча:</p>
							<div class="tabs">
								<input
									type="radio"
									id="static"
									name="ballColorMode"
									value="static"
									v-model="gameSettings.ballColorMode"
								/>
								<label class="tab" for="static">Статический</label>

								<input
									type="radio"
									id="random"
									name="ballColorMode"
									value="random"
									v-model="gameSettings.ballColorMode"
								/>
								<label class="tab" for="random">Рандом</label>

								<input
									type="radio"
									id="gradient"
									name="ballColorMode"
									value="gradient"
									v-model="gameSettings.ballColorMode"
								/>
								<label class="tab" for="gradient">Градиент</label>

								<span class="glider"></span>
							</div>
						</div>

						<div class="setting-item">
							<p for="ballColor">Цвет мяча:</p>
							<input
								type="color"
								id="ballColor"
								v-model="gameSettings.ballColor"
							/>
						</div>
						<div class="setting-item">
							<p for="ballGradientType">Тип градиента мяча:</p>
							<div class="tabs">
								<input
									type="radio"
									id="ball-linear"
									name="ballGradientType"
									value="linear"
									v-model="gameSettings.ballGradientType"
								/>
								<label class="tab" for="ball-linear">Линейный</label>

								<input
									type="radio"
									id="ball-radial"
									name="ballGradientType"
									value="radial"
									v-model="gameSettings.ballGradientType"
								/>
								<label class="tab" for="ball-radial">Радиальный</label>

								<input
									type="radio"
									id="ball-conic"
									name="ballGradientType"
									value="conic"
									v-model="gameSettings.ballGradientType"
								/>
								<label class="tab" for="ball-conic">Конический</label>

								<span class="glider"></span>
							</div>
						</div>

						<div class="setting-item">
							<p for="ballGradientStartColor">Начальный цвет градиента мяча:</p>
							<input
								type="color"
								id="ballGradientStartColor"
								v-model="gameSettings.ballGradientStartColor"
							/>
						</div>
						<div class="setting-item">
							<p for="ballGradientEndColor">Конечный цвет градиента мяча:</p>
							<input
								type="color"
								id="ballGradientEndColor"
								v-model="gameSettings.ballGradientEndColor"
							/>
						</div>
						<div class="setting-item">
							<button @click="toggleSetting('ballGlow')">
								Свечение мяча: {{ gameSettings.ballGlow }}
							</button>
						</div>
						<div class="setting-item">
							<p for="ballGlowColor">Цвет свечения мяча:</p>
							<input
								type="color"
								id="ballGlowColor"
								v-model="gameSettings.ballGlowColor"
							/>
						</div>
						<div class="setting-item">
							<p for="ballGlowBlur">Радиус размытия свечения мяча:</p>
							<input
								type="range"
								id="ballGlowBlur"
								v-model="gameSettings.ballGlowBlur"
								min="0"
								max="50"
							/>
							<span>{{ gameSettings.ballGlowBlur }}</span>
						</div>
					</div>
				</transition>
			</div>
			<div class="section-background section">
				<h1 class="sec-text">Фон</h1>

				<!-- Включение эффектов -->
				<div class="setting-item">
					<button @click="toggleSetting('enableEffects')">
						Включение эффектов фона: {{ gameSettings.enableEffects }}
					</button>
				</div>

				<!-- Выбор типа эффекта -->
				<div class="setting-item">
					<p for="effectType">Тип эффекта:</p>
					<div class="select-wrapper">
						<select
							class="select"
							v-model="gameSettings.effectType"
							id="effectType"
						>
							<option value="particles">Партиклы</option>
							<option value="special" disabled>
								Специальный &#x1F534; TEST
							</option>
							<option value="advanced" disabled>
								Продвинутый &#x1F534; TEST
							</option>
						</select>
					</div>
				</div>

				<!-- Выбор типа фона -->
				<div class="setting-item">
					<p for="backgroundType">Тип фона:</p>
					<div class="select-wrapper">
						<select
							class="select"
							v-model="gameSettings.backgroundType"
							id="backgroundType"
						>
							<option value="solid">Сплошной</option>
							<option value="gradient">Градиент</option>
							<option value="image" disabled>Картинка &#x1F534; TEST</option>
						</select>
					</div>
				</div>

				<!-- Если выбран сплошной фон -->
				<div
					class="setting-item"
					v-if="gameSettings.backgroundType === 'solid'"
				>
					<p for="solidColor">Цвет фона:</p>
					<input
						type="color"
						v-model="gameSettings.solidColor"
						id="solidColor"
					/>
				</div>

				<!-- Если выбран градиент -->
				<div
					class="setting-item"
					v-if="gameSettings.backgroundType === 'gradient'"
				>
					<p for="gradientTypeBackground">Тип градиента:</p>
					<div class="select-wrapper">
						<select
							class="select"
							v-model="gameSettings.gradientTypeBackground"
							id="gradientTypeBackground"
						>
							<option value="linear">Линейный</option>
							<option value="radial">Радиальный</option>
							<option value="conic">Конический</option>
						</select>
					</div>
				</div>

				<div
					class="setting-item"
					v-if="gameSettings.backgroundType === 'gradient'"
				>
					<p for="gradientColor1">Цвет 1:</p>
					<input
						type="color"
						v-model="gameSettings.gradientColor1"
						id="gradientColor1"
					/>

					<p for="gradientColor2">Цвет 2:</p>
					<input
						type="color"
						v-model="gameSettings.gradientColor2"
						id="gradientColor2"
					/>
				</div>

				<div
					class="setting-item"
					v-if="gameSettings.backgroundType === 'gradient'"
				>
					<p for="gradientDirection">Направление:</p>
					<div class="select-wrapper">
						<select
							class="select"
							v-model="gameSettings.gradientDirection"
							id="gradientDirection"
						>
							<option value="to right">Справа налево</option>
							<option value="to left">Слева направо</option>
							<option value="to bottom">Сверху вниз</option>
							<option value="to top">Снизу вверх</option>
						</select>
					</div>
				</div>

				<!-- Если выбрана картинка -->
				<div
					class="setting-item"
					v-if="gameSettings.backgroundType === 'image'"
				>
					<p for="backgroundImage">Фон-картинка:</p>
					<input type="file" @change="uploadImage" />
				</div>
			</div>
		</div>
	</div>
	<div>
		<div class="game-container">
			<canvas ref="gameCanvas" id="gameCanvas"></canvas>
			<canvas ref="backgroundCanvas" id="backgroundCanvas"></canvas>
		</div>
	</div>
</template>

<script>
import { onMounted, ref, nextTick, watch } from 'vue';
import Matter from 'matter-js';

export default {
	setup() {
		const settingsMenu = ref(null);

		// Перед анимацией появления
		const beforeEnter = el => {
			el.style.maxHeight = '0px';
			el.style.opacity = '0';
		};

		// Анимация появления
		const enter = el => {
			nextTick(() => {
				el.style.maxHeight = el.scrollHeight + 'px';
				el.style.opacity = '1';
			});
		};

		// Анимация исчезновения
		const leave = el => {
			el.style.maxHeight = '0px';
			el.style.opacity = '0';
		};

		const isMenuOpen = ref(false);
		const activeTab = ref('');

		const toggleMenu = () => {
			isMenuOpen.value = !isMenuOpen.value;
			console.log('Меню открыто:', isMenuOpen.value); // Выводим состояние меню в консоль
		};

		// Метод для переключения вкладок
		const toggletabs = tab => {
			if (activeTab.value === tab) {
				activeTab.value = ''; // Если вкладка уже открыта, закрыть её
			} else {
				activeTab.value = tab; // Открыть выбранную вкладку
			}
		};

		const toggleSetting = setting => {
			switch (setting) {
				case 'numRings':
					gameSettings.value.numRings++;
					break;
				case 'ringGap':
					gameSettings.value.ringGap += 5;
					break;
				case 'ringThickness':
					gameSettings.value.ringThickness += 1;
					break;
				case 'ringRotationSpeed':
					gameSettings.value.ringRotationSpeed =
						parseFloat(gameSettings.value.ringRotationSpeed) + 1;
					break;
				case 'gapWidth':
					gameSettings.value.gapWidth =
						parseFloat(gameSettings.value.gapWidth) + 1;
					break;
				case 'ballRadius':
					gameSettings.value.ballRadius =
						parseFloat(gameSettings.value.ballRadius) + 1;
					gameSettings.value.ballInitialRadius = gameSettings.value.ballRadius;
					break;
				case 'gravity':
					gameSettings.value.gravity =
						parseFloat(gameSettings.value.gravity) + 1;
					break;
				case 'bounceSpeed':
					gameSettings.value.bounceSpeed += 1;
					break;
				case 'shrinkingRing':
					gameSettings.value.shrinkingRing = !gameSettings.value.shrinkingRing;
					break;
				case 'moveOnlyCurrentRing':
					gameSettings.value.moveOnlyCurrentRing =
						!gameSettings.value.moveOnlyCurrentRing;
					break;
				case 'gshrinkingSpeedravity':
					gameSettings.value.shrinkingSpeed =
						parseFloat(gameSettings.value.shrinkingSpeed) + 10;
					break;
				case 'minRingRadius':
					gameSettings.value.minRingRadius += 5;
					break;

				case 'gradientType':
					// Циклически меняем тип градиента
					this.gameSettings.gradientType =
						this.gameSettings.gradientType === 'radial'
							? 'linear'
							: this.gameSettings.gradientType === 'linear'
							? 'conic'
							: 'radial';
					break;
				case 'ringColorMode':
					// Циклически меняем режим цвета колец
					this.gameSettings.ringColorMode =
						this.gameSettings.ringColorMode === 'static'
							? 'random'
							: this.gameSettings.ringColorMode === 'random'
							? 'gradient'
							: 'static';
					break;
				case 'staticRingColor':
					// Устанавливаем статический цвет колец
					this.gameSettings.staticRingColor =
						this.gameSettings.staticRingColor === '#ff0000'
							? '#00ff00'
							: '#ff0000';
					break;
				case 'ballGradientStartColor':
					this.gameSettings.ballGradientStartColor =
						this.gameSettings.ballGradientStartColor === '#ff0000'
							? '#00ff00'
							: '#ff0000';
					break;
				case 'ballGradientEndColor':
					this.gameSettings.ballGradientEndColor =
						this.gameSettings.ballGradientEndColor === '#ff0000'
							? '#00ff00'
							: '#ff0000';
					break;
				case 'ringGlowColor':
					this.gameSettings.ringGlowColor = getRandomColor();
					break;
				case 'ringGlowBlur':
					gameSettings.value.ringGlowBlur += 1;
					break;
				case 'gradientStartRadius':
					// Увеличиваем начальный радиус градиента
					this.gameSettings.gradientStartRadius += 5;
					break;

				case 'gradientEndRadius':
					// Увеличиваем конечный радиус градиента
					this.gameSettings.gradientEndRadius += 5;
					break;

				case 'gradientCenterX':
					// Увеличиваем координату X центра градиента
					this.gameSettings.gradientCenterX += 10;
					break;

				case 'gradientCenterY':
					// Увеличиваем координату Y центра градиента
					this.gameSettings.gradientCenterY += 10;
					break;

				case 'gradientStartColor':
					// Меняем начальный цвет градиента на случайный
					this.gameSettings.gradientStartColor = getRandomColor();
					break;

				case 'gradientEndColor':
					// Меняем конечный цвет градиента на случайный
					this.gameSettings.gradientEndColor = getRandomColor();
					break;
				case 'ballGlow':
					gameSettings.value.ballGlow = !gameSettings.value.ballGlow;
					break;
				case 'ballColor':
					gameSettings.value.ballColor =
						gameSettings.value.ballColor === '#FF0000' ? '#00FF00' : '#FF0000';
					break;
				case 'ballGlowColor':
					gameSettings.value.ballGlowColor =
						gameSettings.value.ballGlowColor === '#FF0000'
							? '#00FF00'
							: '#FF0000';
					break;
				case 'ballGlowBlur':
					gameSettings.value.ballGlowBlur += 1;
					break;
				case 'ballGradientType': {
					const types = ['linear', 'radial', 'conic'];
					const currentIndex = types.indexOf(
						this.gameSettings.ballGradientType
					);
					this.gameSettings.ballGradientType =
						types[(currentIndex + 1) % types.length];
					break;
				}
				case 'ballColorMode': {
					const types = ['static', 'random', 'gradient'];
					const currentIndex = types.indexOf(this.gameSettings.ballColorMode);
					this.gameSettings.ballColorMode =
						types[(currentIndex + 1) % types.length];
					break;
				}
				case 'ballRadiusIncreaseEnabled':
					gameSettings.value.ballRadiusIncreaseEnabled =
						!gameSettings.value.ballRadiusIncreaseEnabled;
					break;
				case 'ringGlow':
					gameSettings.value.ringGlow = !gameSettings.value.ringGlow;
					break;
				case 'enableEffects':
					gameSettings.value.enableEffects = !gameSettings.value.enableEffects;
					break;
				case 'effectType': {
					const types = ['particles', 'special', 'advanced'];
					const currentIndex = types.indexOf(this.gameSettings.effectType);
					this.gameSettings.effectType =
						types[(currentIndex + 1) % types.length];
					break;
				}
				case 'backgroundType':
					this.gameSettings.backgroundType =
						this.gameSettings.backgroundType === 'solid'
							? 'gradient'
							: this.gameSettings.backgroundType === 'gradient'
							? 'image'
							: 'solid';
					break;
				case 'gradientTypeBackground':
					this.gameSettings.gradientTypeBackground =
						this.gameSettings.gradientTypeBackground === 'linear'
							? 'radial'
							: this.gameSettings.gradientTypeBackground === 'radial'
							? 'conic'
							: 'linear';
					break;
				default:
					break;
			}
		};
		function getRandomColor() {
			const letters = '0123456789ABCDEF';
			let color = '#';
			for (let i = 0; i < 6; i++) {
				color += letters[Math.floor(Math.random() * 16)];
			}
			return color;
		}

		const gameCanvas = ref(null);
		const backgroundCanvas = ref(null);

		let engine, world, ball;
		let rings = [];
		const currentRing = ref(null); // Текущее активное кольцо
		let isManualSizeChange = false; // Флаг для отслеживания ручного изменения

		const gameSettings = ref({
			numRings: 10,
			ringGap: 20,
			ringThickness: 5,
			ringRotationSpeed: 5, // Общая скорость вращения для всех колец
			gapWidth: 30, // Щель всегда статична

			ballRadius: 15,
			ballInitialRadius: 15,

			gravity: 15,
			bounceSpeed: 3,

			shrinkingRing: false, // Включить уменьшение кольца с мячом
			moveOnlyCurrentRing: false, // Двигается только текущее кольцо
			shrinkingSpeed: 80, // Скорость уменьшения кольца
			minRingRadius: 30, // Минимальный радиус кольца

			gradientCenterX: window.innerWidth / 0.9, // Центр градиента по оси X
			gradientCenterY: window.innerHeight / 3.5, // Центр градиента по оси Y
			gradientStartRadius: 100, // Начальный радиус
			gradientEndRadius: Math.max(window.innerWidth, window.innerHeight), // Конечный радиус

			gradientType: 'radial', // Тип градиента: 'linear', 'radial', 'conic'
			ringColorMode: 'static', // Возможные значения: 'static', 'random', 'gradient'

			staticRingColor: '#00C3FFFF', // Статический цвет для колец

			gradientStartColor: '#FF0000', // Начальный цвет для градиента
			gradientEndColor: '#0000FF', // Конечный цвет для градиента

			gradientRingColors: ['#FF0000', '#00FF00', '#0000FF'], // Массив цветов для градиента кольца
			randomRingColors: ['#FF0000', '#00FF00', '#0000FF'], // Массив цветов для случайного выбора

			ringGlow: false, // Включить свечение колец
			ringGlowColor: '#29819C93', // Цвет свечения колец
			ringGlowBlur: 15, // Радиус размытия свечения

			ballColor: '#FF0000', // Статичный цвет мяча, по умолчанию красный

			ballGlow: false, // Включить свечение мяча
			ballGlowColor: '#00FFFF', // Цвет свечения мяча
			ballGlowBlur: 35, // Радиус размытия свечения мяча

			ballGradientType: 'radial', // Тип градиента для мяча: 'linear', 'radial', 'conic'

			ballGradientStartColor: '#FF0000', // Начальный цвет для градиента мяча
			ballGradientEndColor: '#0000FF', // Конечный цвет для градиента мяча

			ballColorMode: 'static', // Режим цвета мяча: 'static', 'random', 'gradient'

			ballRadiusIncreaseEnabled: false, // Включить увеличение радиуса мяча
			ballRadiusIncreaseRate: 0.1, // Скорость увеличения радиуса мяча

			// Настройки смещения колец
			ringOffsetX: 0, // Смещение колец по оси X
			ringOffsetY: 0, // Смещение колец по оси Y

			// Дополнительные эффекты
			enableEffects: false, // Включить эффекты фона
			effectType: 'particles', // Тип эффектов: 'particles', 'special', 'advanced'
			backgroundType: 'solid', // solid | gradient | image
			solidColor: '#000000',
			gradientTypeBackground: 'linear', // linear | radial | conic
			gradientColor1: '#ff0000',
			gradientColor2: '#0000ff',
			gradientDirection: 'to right',
			backgroundImage: null,

			// Тест
			// ringDestructionType: 'explode', // Тип разрушения колец: 'shrink', 'explode', 'disappear'
			// enableBallEffects: false, // Включение эффектов от мяча (например, гостинг)
			// ballGhostingEffect: false, // Включить эффект гостинга мяча (множество мячей или следы)

			// // Настройки разрушения
			// destroyedRingColor: '#FF0000', // Цвет разрушенного кольца
			// shrinkSpeed: 50, // Скорость сжатия кольца при разрушении
			// explodeSpeed: 1.5, // Скорость взрыва колец
		});

		// const updateRingDestruction = ring => {
		// 	if (gameSettings.ringDestructionType === 'shrink') {
		// 		// Просто убираем кольцо без эффектов
		// 		rings = rings.filter(r => r !== ring);
		// 	} else if (gameSettings.ringDestructionType === 'explode') {
		// 		// Взрыв кольца — создаем частицы
		// 		console.log('взрыв');
		// 		if (ring && !ring.exploded) {
		// 			ring.exploded = true; // Отметка о том, что кольцо взорвалось
		// 			createExplosionEffect(ring);
		// 		}

		// 		// Удалить кольцо через некоторое время, после взрыва
		// 		setTimeout(() => {
		// 			rings = rings.filter(r => r !== ring);
		// 		}, 500); // Удаляем через 500 мс
		// 	} else if (gameSettings.ringDestructionType === 'disappear') {
		// 		// Эффект исчезновения кольца
		// 		rings = rings.filter(r => r !== ring);
		// 	}
		// };

		// // Функция создания эффекта взрыва (рассыпание кольца)
		// const createExplosionEffect = ring => {
		// 	const particleCount = 50; // Количество частиц
		// 	const particles = [];

		// 	// Генерация частиц для взрыва
		// 	for (let i = 0; i < particleCount; i++) {
		// 		const angle = Math.random() * Math.PI * 2; // случайный угол
		// 		const speed = Math.random() * 5 + 5; // случайная скорость
		// 		const xVelocity = Math.cos(angle) * speed; // скорость по X
		// 		const yVelocity = Math.sin(angle) * speed; // скорость по Y

		// 		// Каждая частица — это объект с позицией, скоростью и временем жизни
		// 		particles.push({
		// 			x: ring.x,
		// 			y: ring.y,
		// 			xVelocity: xVelocity,
		// 			yVelocity: yVelocity,
		// 			life: 1, // Время жизни частицы
		// 		});
		// 	}

		// 	// Добавление частиц в игровой процесс
		// 	animateExplosion(particles);
		// };

		// // Функция анимации взрыва
		// const animateExplosion = particles => {
		// 	// Здесь будет отрисовываться каждая частица в процессе их движения
		// 	// Обновляем позиции частиц
		// 	particles.forEach(particle => {
		// 		particle.x += particle.xVelocity;
		// 		particle.y += particle.yVelocity;
		// 		particle.life -= 0.02; // уменьшение жизни частицы

		// 		// Рисуем частицы
		// 		drawParticle(particle); // Функция отрисовки частицы

		// 		// Удаляем частицы, если их жизнь закончилась
		// 		if (particle.life <= 0) {
		// 			particles = particles.filter(p => p !== particle);
		// 		}
		// 	});
		// };

		// // Функция отрисовки частицы
		// const drawParticle = (ctx, particle) => {
		// 	ctx.beginPath();
		// 	ctx.arc(particle.x, particle.y, 3, 0, Math.PI * 2);
		// 	ctx.fillStyle = 'rgba(255, 0, 0, ' + particle.life + ')';
		// 	ctx.fill();
		// };

		const initGame = () => {
			nextTick(() => {
				const canvas = gameCanvas.value;
				if (canvas) {
					const ctx = canvas.getContext('2d');
					canvas.width = window.innerWidth;
					canvas.height = window.innerHeight;

					// Инициализация Matter.js
					engine = Matter.Engine.create();
					world = engine.world;

					engine.world.gravity.y = gameSettings.value.gravity / 100;

					// Запуск физики
					Matter.Engine.run(engine);

					// Генерация колец и мяча
					rings = generateRings();
					ball = generateBall();

					// Добавление мяча в мир
					Matter.World.add(world, ball);

					// Анимация игры
					animate(ctx);

					// Запуск эффекта фона, если он включен
					if (gameSettings.value.enableEffects) {
						startBackgroundEffects();
					}
				} else {
					console.error('Canvas element is still null');
				}
			});
		};

		const generateRings = () => {
			// Проверяем актуальные значения и округляем скорость вращения
			const roundedRingRotationSpeed = Math.round(
				gameSettings.value.ringRotationSpeed
			);

			const ringsArray = [];
			const baseAngle = Math.random() * Math.PI * 2;

			for (let i = 0; i < gameSettings.value.numRings; i++) {
				const radius = 100 + i * gameSettings.value.ringGap;
				ringsArray.push({
					radius,
					initialRadius: radius,
					angle: baseAngle + (i * Math.PI) / 8,
					initialAngle: baseAngle + (i * Math.PI) / 8,
					angleSpeed: roundedRingRotationSpeed, // Используем округленную скорость
				});
			}

			return ringsArray;
		};

		const generateBall = () => {
			const speed = 4 + Math.random() * 4;
			const angle = Math.random() * Math.PI * 2;
			const ball = Matter.Bodies.circle(
				window.innerWidth / 2,
				window.innerHeight / 2,
				gameSettings.value.ballRadius,
				{
					restitution: 0.5,
					friction: 0,
					frictionAir: 0,
					inertia: Infinity,
					render: { fillStyle: 'red' },
				}
			);
			Matter.Body.setVelocity(ball, {
				x: Math.cos(angle) * speed,
				y: Math.sin(angle) * speed,
			});
			return ball;
		};

		const angleDifference = (a, b) => {
			let diff = a - b;
			while (diff < -Math.PI) diff += Math.PI * 2;
			while (diff > Math.PI) diff -= Math.PI * 2;
			return diff;
		};

		const checkRingCollisions = () => {
			const center = { x: window.innerWidth / 2, y: window.innerHeight / 2 };
			const ballPos = ball.position;
			const dx = ballPos.x - center.x;
			const dy = ballPos.y - center.y;
			const d = Math.hypot(dx, dy);
			const ballAngle = Math.atan2(dy, dx);

			const minCollisionDist = ring =>
				ring.radius -
				gameSettings.value.ringThickness / 2 -
				gameSettings.value.ballRadius;
			const maxCollisionDist = ring =>
				ring.radius +
				gameSettings.value.ringThickness / 2 +
				gameSettings.value.ballRadius;

			rings = rings.filter(ring => {
				if (d >= minCollisionDist(ring) && d <= maxCollisionDist(ring)) {
					const diff = Math.abs(angleDifference(ballAngle, ring.angle));

					if (diff < gameSettings.value.gapWidth / 100) {
						// Делаем дробное
						return false;
					} else {
						let n = { x: dx / d, y: dy / d };
						if (d < ring.radius) {
							n = { x: -n.x, y: -n.y };
						}
						const dot = ball.velocity.x * n.x + ball.velocity.y * n.y;
						if (dot < 0) {
							let reflectedX = ball.velocity.x - 2 * dot * n.x;
							let reflectedY = ball.velocity.y - 2 * dot * n.y;

							const bounceFactor = 1 + ring.radius / 200;
							reflectedX *= bounceFactor;
							reflectedY *= bounceFactor;

							const randomAngleOffset = ((Math.random() - 0.5) * Math.PI) / 6;
							const angle =
								Math.atan2(reflectedY, reflectedX) + randomAngleOffset;
							reflectedX = Math.cos(angle) * gameSettings.value.bounceSpeed;
							reflectedY = Math.sin(angle) * gameSettings.value.bounceSpeed;
							Matter.Body.setVelocity(ball, { x: reflectedX, y: reflectedY });
						}
					}
				}

				return true;
			});
			// Проверяем, уменьшилось ли количество колец
			// if (rings.length < initialRingsLength) {
			// 	const destroyedRing = rings[initialRingsLength - 1]; // Последнее удаленное кольцо
			// 	updateRingDestruction(destroyedRing); // Вызов функции разрушения кольца
			// }

			if (rings.length === 0) {
				restartGame();
			}
		};

		const drawBall = ctx => {
			let ballColor;

			// Если активирован эффект гостинга мяча
			if (gameSettings.value.ballGhostingEffect) {
				// Создать несколько мячей с прозрачностью
				for (let i = 0; i < 5; i++) {
					const alpha = 1 - i * 0.2; // Снижаем прозрачность
					ctx.globalAlpha = alpha;
					ctx.fillStyle = gameSettings.value.ballColor || '#FF0000';
					ctx.beginPath();
					ctx.arc(
						ball.position.x + i * 10, // Смещение мячей
						ball.position.y + i * 10,
						gameSettings.value.ballRadius,
						0,
						Math.PI * 2
					);
					ctx.fill();
				}
				ctx.globalAlpha = 1; // Возвращаем нормальную прозрачность
			} // Логика выбора цвета мяча в зависимости от режима
			else if (gameSettings.value.ballColorMode === 'random') {
				// Случайный цвет
				ballColor = `hsl(${Math.random() * 360}, 100%, 50%)`;
			} else if (gameSettings.value.ballColorMode === 'gradient') {
				// Градиент для мяча
				let gradient;
				const canvasWidth = window.innerWidth;
				const canvasHeight = window.innerHeight;
				if (gameSettings.value.ballGradientType === 'linear') {
					gradient = ctx.createLinearGradient(0, 0, canvasWidth, canvasHeight);
					gradient.addColorStop(0, gameSettings.value.ballGradientStartColor);
					gradient.addColorStop(1, gameSettings.value.ballGradientEndColor);
				} else if (gameSettings.value.ballGradientType === 'radial') {
					gradient = ctx.createRadialGradient(
						ball.position.x,
						ball.position.y,
						0,
						ball.position.x,
						ball.position.y,
						gameSettings.value.ballRadius
					);
					gradient.addColorStop(0, gameSettings.value.ballGradientStartColor);
					gradient.addColorStop(1, gameSettings.value.ballGradientEndColor);
				} else if (gameSettings.value.ballGradientType === 'conic') {
					// Конический градиент (если поддерживается)
					gradient = ctx.createConicGradient(
						0,
						ball.position.x,
						ball.position.y
					);
					gradient.addColorStop(0, gameSettings.value.ballGradientStartColor);
					gradient.addColorStop(1, gameSettings.value.ballGradientEndColor);
				}
				ballColor = gradient;
			} else if (gameSettings.value.ballColorMode === 'static') {
				// Статичный цвет мяча
				ballColor = gameSettings.value.ballColor || '#FF0000'; // Если ballColor не задан, используем дефолтный красный
			}

			// Применяем эффект свечения, если он включен
			if (gameSettings.value.ballGlow) {
				ctx.shadowColor = gameSettings.value.ballGlowColor;
				ctx.shadowBlur = gameSettings.value.ballGlowBlur;
			} else {
				ctx.shadowColor = 'transparent'; // Сбрасываем эффект свечения
			}

			// Отрисовка мяча с учетом свечения
			ctx.fillStyle = ballColor;
			ctx.beginPath();
			ctx.arc(
				ball.position.x,
				ball.position.y,
				gameSettings.value.ballRadius, // Используем радиус
				0,
				Math.PI * 2
			);
			ctx.fill();

			// Если игра окончена (выигрыш), сбрасываем радиус мяча
			if (gameSettings.value.gameOver || rings.length === 0) {
				gameSettings.value.ballRadius = gameSettings.value.ballInitialRadius; // Сбрасываем радиус
			} else if (gameSettings.value.ballRadiusIncreaseEnabled) {
				// Если включено увеличение радиуса, увеличиваем его
				gameSettings.value.ballRadius +=
					gameSettings.value.ballRadiusIncreaseRate;
			}

			// Сбрасываем эффект свечения после отрисовки
			ctx.shadowColor = 'transparent';
			ctx.shadowBlur = 0;
		};

		const updateBallSize = () => {
			if (!isManualSizeChange && gameSettings.value.ballRadiusIncreaseEnabled) {
				gameSettings.value.ballRadius +=
					gameSettings.value.ballRadiusIncreaseRate;
			}

			// Если мяч достиг предела или кольца исчезли, сбрасываем его
			if (gameSettings.value.gameOver || rings.length === 0) {
				gameSettings.value.ballRadius = gameSettings.value.ballInitialRadius;
				isManualSizeChange = false; // Сбрасываем флаг
			}

			requestAnimationFrame(updateBallSize);
		};

		// const setBallRadius = newRadius => {
		// 	isManualSizeChange = true;
		// 	gameSettings.value.ballRadius = newRadius;
		// };

		const animate = ctx => {
			ctx.clearRect(0, 0, window.innerWidth, window.innerHeight);
			Matter.Engine.update(engine);

			// Обновляем текущее кольцо (каждый кадр)
			updateCurrentRing();

			drawBall(ctx);
			drawRings(ctx);
			checkRingCollisions(); // Проверка столкновений, но она не влияет на уменьшение радиуса

			requestAnimationFrame(() => animate(ctx));
		};

		const updateCurrentRing = () => {
			const center = { x: window.innerWidth / 2, y: window.innerHeight / 2 };
			const ballPos = ball.position;
			const dx = ballPos.x - center.x;
			const dy = ballPos.y - center.y;
			const distance = Math.hypot(dx, dy);

			currentRing.value = rings.find(ring => {
				const minDistance =
					ring.radius -
					gameSettings.value.ringThickness / 0 -
					gameSettings.value.ballRadius;
				const maxDistance =
					ring.radius +
					gameSettings.value.ringThickness / 2 +
					gameSettings.value.ballRadius;
				return distance >= minDistance && distance <= maxDistance;
			});

			const rotationSpeed = gameSettings.value.ringRotationSpeed / 1000; // Делаем дробное

			if (gameSettings.value.moveOnlyCurrentRing && currentRing.value) {
				currentRing.value.angle += rotationSpeed;
				if (currentRing.value.angle > Math.PI * 2) {
					currentRing.value.angle -= Math.PI * 2;
				}
			} else {
				rings.forEach(ring => {
					ring.angle += rotationSpeed;
					if (ring.angle > Math.PI * 2) {
						ring.angle -= Math.PI * 2;
					}
				});
			}

			if (gameSettings.value.shrinkingRing && currentRing.value) {
				if (currentRing.value.radius > gameSettings.value.minRingRadius) {
					currentRing.value.radius -= gameSettings.value.shrinkingSpeed / 500;
				}
			}
		};

		const drawRings = ctx => {
			const canvasWidth = window.innerWidth;
			const canvasHeight = window.innerHeight;

			rings.forEach(ring => {
				let ringColor;

				// Логика выбора цвета кольца
				if (
					gameSettings.value.ringColorMode === 'random' &&
					gameSettings.value.randomRingColors.length > 0
				) {
					ringColor =
						gameSettings.value.randomRingColors[
							Math.floor(
								Math.random() * gameSettings.value.randomRingColors.length
							)
						];
				} else if (gameSettings.value.ringColorMode === 'gradient') {
					let gradient;
					if (gameSettings.value.gradientType === 'linear') {
						gradient = ctx.createLinearGradient(
							0,
							0,
							canvasWidth,
							canvasHeight
						);
						gradient.addColorStop(0, gameSettings.value.gradientStartColor);
						gradient.addColorStop(1, gameSettings.value.gradientEndColor);
					} else if (gameSettings.value.gradientType === 'radial') {
						gradient = ctx.createRadialGradient(
							gameSettings.value.gradientCenterX,
							gameSettings.value.gradientCenterY,
							gameSettings.value.gradientStartRadius,
							gameSettings.value.gradientCenterX,
							gameSettings.value.gradientCenterY,
							gameSettings.value.gradientEndRadius
						);
						gradient.addColorStop(0, gameSettings.value.gradientStartColor);
						gradient.addColorStop(1, gameSettings.value.gradientEndColor);
					} else if (gameSettings.value.gradientType === 'conic') {
						gradient = ctx.createConicGradient(
							ring.angle,
							canvasWidth / 2,
							canvasHeight / 2
						);
						gradient.addColorStop(0, gameSettings.value.gradientStartColor);
						gradient.addColorStop(1, gameSettings.value.gradientEndColor);
					}
					ringColor = gradient;
				} else {
					ringColor = gameSettings.value.staticRingColor;
				}

				// Применяем эффект свечения, если он включен
				if (gameSettings.value.ringGlow) {
					ctx.shadowColor = gameSettings.value.ringGlowColor;
					ctx.shadowBlur = gameSettings.value.ringGlowBlur;
				} else {
					ctx.shadowColor = 'transparent'; // Сбрасываем эффект свечения
				}

				// Смещение колец
				const centerX = window.innerWidth / 2 + gameSettings.value.ringOffsetX;
				const centerY = window.innerHeight / 2 + gameSettings.value.ringOffsetY;

				// Отрисовка кольца с учетом толщины
				ctx.strokeStyle = ringColor;
				ctx.lineWidth = gameSettings.value.ringThickness;
				ctx.beginPath();
				ctx.arc(
					centerX,
					centerY,
					ring.radius,
					ring.angle + gameSettings.value.gapWidth / 100, // Делен на 100
					ring.angle + Math.PI * 2 - gameSettings.value.gapWidth / 100 // Делен на 100
				);
				ctx.stroke();

				// Сбрасываем эффект свечения после отрисовки
				ctx.shadowColor = 'transparent';
				ctx.shadowBlur = 0;
			});
		};

		const restartGame = () => {
			Matter.World.clear(world);
			rings = generateRings();
			ball = generateBall();
			Matter.World.add(world, ball);
		};

		watch(
			() => gameSettings.value.enableEffect,
			newValue => {
				if (newValue) {
					startBackgroundEffects(); // Включаем фон
				} else {
					stopBackgroundEffects(); // Выключаем фон
				}
			}
		);

		let backgroundAnimationFrame = null; // Для хранения requestAnimationFrame

		const startBackgroundEffects = () => {
			if (!gameSettings.value.enableEffects) {
				stopBackgroundEffects(); // Останавливаем эффекты, если выключены
				return;
			}
			const canvas = backgroundCanvas.value;
			const ctx = canvas.getContext('2d');

			// Инициализация и запуск эффекта частиц
			let particles = [];
			const NUM_PARTICLES = 600;
			const PARTICLE_SIZE = 0.8;

			// Уменьшаем скорость для более медленной анимации
			const SPEED = 40000; // Увеличим скорость, чтобы частицы двигались медленнее

			function createParticle() {
				const colour = { r: 191, g: 226, b: 255, a: 0.2 }; // Уменьшаем альфа-канал, чтобы сделать частицы менее яркими
				return {
					x: -2,
					y: -2,
					diameter: Math.max(0, Math.random() * PARTICLE_SIZE),
					duration: Math.random() * SPEED,
					amplitude: Math.random() * 16,
					offsetY: Math.random() * 10,
					arc: Math.PI * 2,
					startTime: performance.now() - Math.random() * SPEED,
					colour: `rgba(${colour.r}, ${colour.g}, ${colour.b}, ${colour.a})`, // Применяем уменьшенную яркость
				};
			}

			function moveParticle(particle, time) {
				const progress =
					((time - particle.startTime) % particle.duration) / particle.duration;
				return {
					...particle,
					x: progress,
					y:
						Math.sin(progress * particle.arc) * particle.amplitude +
						particle.offsetY,
				};
			}

			function drawParticle(particle) {
				ctx.fillStyle = particle.colour;
				ctx.beginPath();
				ctx.ellipse(
					particle.x * canvas.width,
					particle.y * (canvas.height / 100) + canvas.height / 2,
					particle.diameter * (canvas.height / 100),
					particle.diameter * (canvas.height / 100),
					0,
					0,
					2 * Math.PI
				);
				ctx.fill();
			}

			function draw(time) {
				if (!gameSettings.value.enableEffects) {
					stopBackgroundEffects(); // Прекращаем анимацию, если фон выключен
					return;
				}
				particles.forEach((particle, index) => {
					particles[index] = moveParticle(particle, time);
				});

				// Очистить канвас с прозрачным фоном
				ctx.clearRect(0, 0, canvas.width, canvas.height);

				// Прозрачный фон, делаем его менее ярким, можно использовать полупрозрачный черный
				ctx.fillStyle = 'rgba(0, 0, 0, 0.2)'; // Добавляем легкую полупрозрачность
				ctx.fillRect(0, 0, canvas.width, canvas.height);

				particles.forEach(drawParticle);
				requestAnimationFrame(draw);
			}

			// Инициализация канваса
			canvas.width = window.innerWidth;
			canvas.height = window.innerHeight;

			for (let i = 0; i < NUM_PARTICLES; i++) {
				particles.push(createParticle());
			}

			requestAnimationFrame(draw);
		};

		const stopBackgroundEffects = () => {
			if (backgroundAnimationFrame) {
				cancelAnimationFrame(backgroundAnimationFrame);
				backgroundAnimationFrame = null;
			}

			const canvas = backgroundCanvas.value;
			const ctx = canvas.getContext('2d');
			ctx.clearRect(0, 0, canvas.width, canvas.height);
		};

		onMounted(() => {
			initGame();

			if (gameSettings.value.enableEffects) {
				startBackgroundEffects();
			}
		});

		// Создаём наблюдателя для отслеживания изменений в настройках
		watch(
			() => ({ ...gameSettings.value, ballRadius: undefined }), // Исключаем ballRadius
			(newSettings, oldSettings) => {
				// Если изменения коснулись только ballRadius, не перезапускаем игру
				if (JSON.stringify(newSettings) === JSON.stringify(oldSettings)) {
					return;
				}

				// Проверяем, включен ли фон
				if (gameSettings.value.enableEffects === true) {
					startBackgroundEffects();
				} else {
					stopBackgroundEffects(); // Останавливаем эффекты, если фон выключен
				}

				// Перезапуск игры при изменении других настроек
				restartGame();
				Matter.Engine.run(engine);
			},
			{ deep: true }
		);

		watch(
			() => gameSettings.value.gravity,
			newGravity => {
				if (engine && engine.world) {
					engine.world.gravity.y = newGravity / 100;
					console.log('Gravity updated:', engine.world.gravity.y);
				}
			}
		);
		watch(
			() => gameSettings.value.backgroundType,
			() => updateBackground()
		);

		const updateBackground = () => {
			const canvas = backgroundCanvas.value;
			if (!canvas) return;

			const ctx = canvas.getContext('2d');
			if (!ctx) return;

			// Устанавливаем размеры canvas в соответствии с окном
			canvas.width = window.innerWidth;
			canvas.height = window.innerHeight;

			ctx.clearRect(0, 0, canvas.width, canvas.height);

			if (gameSettings.value.backgroundType === 'solid') {
				ctx.fillStyle = gameSettings.value.solidColor;
				ctx.fillRect(0, 0, canvas.width, canvas.height);
			} else if (gameSettings.value.backgroundType === 'gradient') {
				let gradient;
				const {
					gradientTypeBackground,
					gradientDirection,
					gradientColor1,
					gradientColor2,
				} = gameSettings.value;

				if (gradientTypeBackground === 'linear') {
					// Разбираем направление и задаём координаты
					let x0 = 0,
						y0 = 0,
						x1 = canvas.width,
						y1 = 0;
					if (gradientDirection === 'to right') {
						x0 = 0;
						x1 = canvas.width;
						y0 = y1 = 0;
					} else if (gradientDirection === 'to left') {
						x0 = canvas.width;
						x1 = 0;
						y0 = y1 = 0;
					} else if (gradientDirection === 'to bottom') {
						x0 = x1 = 0;
						y0 = 0;
						y1 = canvas.height;
					} else if (gradientDirection === 'to top') {
						x0 = x1 = 0;
						y0 = canvas.height;
						y1 = 0;
					}
					gradient = ctx.createLinearGradient(x0, y0, x1, y1);
				} else if (gradientTypeBackground === 'radial') {
					gradient = ctx.createRadialGradient(
						canvas.width / 2,
						canvas.height / 2,
						50,
						canvas.width / 2,
						canvas.height / 2,
						Math.max(canvas.width, canvas.height) / 2
					);
				} else if (gradientTypeBackground === 'conic') {
					gradient = ctx.createConicGradient(
						0,
						canvas.width / 2,
						canvas.height / 2
					);
				}

				gradient.addColorStop(0, gradientColor1);
				gradient.addColorStop(1, gradientColor2);
				ctx.fillStyle = gradient;
				ctx.fillRect(0, 0, canvas.width, canvas.height);
			} else if (
				gameSettings.value.backgroundType === 'image' &&
				gameSettings.value.backgroundImage
			) {
				const img = new Image();
				img.src = gameSettings.value.backgroundImage;
				img.crossOrigin = 'anonymous'; // Если изображение загружается из другого источника
				img.onload = () => {
					ctx.clearRect(0, 0, canvas.width, canvas.height); // Очищаем перед обновлением
					ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
				};
				img.onerror = err => console.error('Ошибка загрузки изображения:', err);
			}
		};

		// Обновляем фон при изменении настроек
		watch(gameSettings, updateBackground, { deep: true });

		// Обновляем при изменении размера окна
		window.addEventListener('resize', updateBackground);

		return {
			gameSettings,
			gameCanvas,
			backgroundCanvas,
			toggleSetting,
			toggleMenu,
			isMenuOpen,
			toggletabs,
			activeTab,
			updateBallSize,
			leave,
			enter,
			settingsMenu,
			beforeEnter,
		};
	},
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Rubik:ital,wght@0,300..900;1,300..900&display=swap');
* {
	font-family: 'Rubik', serif;
	font-optical-sizing: auto;
	color: aliceblue;
}

body {
	font-family: 'Rubik', serif;
	background-color: black;
	margin: 0;
	padding: 0;
}
/* Контейнер для канвасов */
.game-container {
	position: absolute;
	width: 100%;
	height: 100vh; /* Используем 100% высоты окна браузера */
	overflow: hidden;
}

/* Общие стили для канвасов */
canvas {
	border: none;
	position: absolute;
}

/* Канвас фона */
#backgroundCanvas {
	z-index: 0; /* Помещаем на нижний слой */
}

/* Игровой канвас */
#gameCanvas {
	z-index: 1; /* Помещаем на верхний слой */
}

/* Контейнер кнопки */
.burger-btn {
	position: absolute;
	top: 42px;
	left: 23px;
	width: 70px;
	height: 70px;
	background: none;
	border: none;
	cursor: pointer;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	align-items: flex-start; /* Выравнивание слева */
	z-index: 1000;
	background: rgba(223, 223, 223, 0.329);
	box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
	backdrop-filter: blur(7.1px);
	-webkit-backdrop-filter: blur(7.1px);
	border: 2px solid rgba(255, 255, 255, 0.44);
	border-radius: 22px;
	padding: 14px;
	-webkit-box-shadow: 0px 10px 127px -4px rgba(0, 0, 0, 0.75);
	-moz-box-shadow: 0px 10px 127px -4px rgba(0, 0, 0, 0.75);
	box-shadow: 0px 10px 127px -4px rgba(0, 0, 0, 0.75);
}

/* Скрываем чекбокс, но оставляем его активным */
.burger-btn input {
	position: absolute;
	width: 100%;
	height: 100%;
	opacity: 0;
	cursor: pointer;
	z-index: 2; /* Поверх спанов */
}

/* Базовый стиль линий */
.burger-btn span {
	display: block;
	height: 6px;
	background: white;
	border-radius: 5px;
	transition: transform 0.6s cubic-bezier(0.25, 1.5, 0.5, 1), opacity 0.4s ease,
		width 0.6s cubic-bezier(0.25, 1.5, 0.5, 1);
}

/* Индивидуальные классы для каждого span */
.burger-btn .line-1 {
	width: 50%;
	transform-origin: left;
}

.burger-btn .line-2 {
	width: 100%;
}

.burger-btn .line-3 {
	width: 75%;
	transform-origin: left;
}

/* Анимация при активации */
.burger-btn input:checked ~ .line-1 {
	transform: rotate(45deg) translate(7px, 0px);
	width: 84%;
	margin-left: 3px;
}

.burger-btn input:checked ~ .line-2 {
	opacity: 0;
	margin-left: 3px;
}

.burger-btn input:checked ~ .line-3 {
	transform: rotate(-45deg) translate(7px, 0px);
	width: 84%;
	margin-left: 3px;
}

.settings-menu {
	overflow: hidden;
	max-height: 0;
	margin-top: 20px;
	opacity: 0;
	transition: max-height 0.6s cubic-bezier(0.25, 1, 0.5, 1),
		opacity 0.4s ease-in-out;
}

/* Анимация появления */
.slide-enter-active {
	max-height: 100vh; /* Можно больше, если контент большой */
	opacity: 1;
}

/* Анимация исчезновения */
.slide-leave-active {
	max-height: 0;
	opacity: 0;
}

/* Меню */
.menu {
	position: fixed;
	top: 0;
	left: -100%; /* Начинается за пределами экрана */
	width: 400px; /* Ширина меню */
	height: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	z-index: 100;
	transition: left 0.5s cubic-bezier(0.25, 1, 0.5, 1);
}

/* Показываем меню */
.menu.show {
	left: 0;
}

.settings {
	background: rgba(223, 223, 223, 0.329);
	box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
	backdrop-filter: blur(20.1px);
	-webkit-backdrop-filter: blur(20.1px);
	border: 2px solid rgba(255, 255, 255, 0.44);
	border-radius: 22px;
	padding: 15px;
	width: 80%;
	height: 88%;
	max-width: 600px;
	max-height: 92%;
	overflow-y: auto; /* Вертикальная прокрутка */
}

.setting-item {
	margin-bottom: 30px;
	margin-top: -10px;
}
.setting-item span {
	padding-left: 5px;
	color: #a7a7a7;
}

.setting-item button {
	width: 100%;
	background-color: rgba(116, 116, 116, 0.562);
	color: white;
	padding: 10px;
	border: none;
	border-radius: 8px;
	cursor: pointer;
	transition: background-color 0.3s ease;
}

.setting-item button:hover {
	background-color: rgba(255, 255, 255, 0.3);
}
.setting-item p {
	color: #c2c2c2ff;
	font-size: 16px !important;
}
.background-preview {
	width: 100%;
	height: 150px;
	border: 1px solid #ccc;
	margin-top: 10px;
	border-radius: 8px;
}

.text-settings {
	padding-left: 70px;
	font-size: 27px;
	margin-top: 10px;
}

/* Стилизация скроллбара (для WebKit-браузеров: Chrome, Edge, Safari) */
.settings::-webkit-scrollbar {
	width: 12px; /* Сделал толще */
}

.settings::-webkit-scrollbar-thumb {
	background: rgba(255, 255, 255, 0.3);
	border-radius: 10px; /* Закругленные края */
}

.settings::-webkit-scrollbar-thumb:hover {
	background: rgba(255, 255, 255, 0.6);
}

.settings::-webkit-scrollbar-track {
	background: rgba(0, 0, 0, 0.2); /* Фон трека */
	border-radius: 10px;
	margin: 15px;
}
.sec-text {
	font-size: 25px;
	margin-top: 35px;
}
.section-header {
	margin-top: 10px;
	width: 275px;
	background: rgba(255, 255, 255, 0.1);
	padding: 12px 16px;
	border-radius: 10px;
	display: flex;
	justify-content: space-between;
	align-items: center;
	cursor: pointer;
	transition: background 0.3s ease;
}
.section-header span {
	color: #cfcfcfff;
}
.section-header:hover {
	background: rgba(255, 255, 255, 0.2);
}

/* Иконка caret */
.caret-icon {
	display: inline-block;
	width: 12px;
	height: 12px;
	border-right: 2px solid #cfcfcfff;
	border-bottom: 2px solid #cfcfcfff;
	transform: rotate(45deg);
	margin-top: -4px;
	transition: transform 0.3s ease-in-out;
}

/* Переворот иконки при активном состоянии */
.rotated {
	transform: rotate(225deg);
}
.settings-menu {
	padding: 10px;
	border-radius: 13px;
	background-color: rgba(0, 0, 0, 0.2);
}

input[type='range'] {
	outline: 0;
	border: 0;
	border-radius: 500px;
	width: 250px;
	max-width: 100%;
	transition: box-shadow 0.2s ease-in-out;

	@media screen and (-webkit-min-device-pixel-ratio: 0) {
		& {
			overflow: hidden;
			height: 20px;
			-webkit-appearance: none;
			background-color: #ddd;
		}
		&::-webkit-slider-runnable-track {
			height: 20px;
			-webkit-appearance: none;
			color: #444;
			transition: box-shadow 0.2s ease-in-out;
		}
		&::-webkit-slider-thumb {
			width: 20px;
			-webkit-appearance: none;
			height: 20px;
			cursor: ew-resize;
			background: #fff;
			box-shadow: -332px 0 0 320px #1597ff, inset 0 0 0 40px #686868ff;
			border-radius: 50%;
			transition: box-shadow 0.2s ease-in-out;
			position: relative;
		}
		&:active::-webkit-slider-thumb {
			background: #fff;
			box-shadow: -332px 0 0 320px #6aace2ff, inset 0 0 0 3px #414141ff;
		}
	}
}

.setting-menu-dop p {
	padding-top: 10px;
	font-size: 20px;
}
.TEST {
	color: RED;
	text-align: center;
	padding-top: 45px;
}

.tabs {
	display: flex;
	position: relative;
	background-color: #fff;
	box-shadow: 0 0 1px 0 rgba(24, 94, 224, 0.15),
		0 6px 12px 0 rgba(24, 94, 224, 0.15);
	padding: 0.75rem;
	border-radius: 99px;
	width: fit-content;
}

.tabs * {
	z-index: 2;
}

.tabs input[type='radio'] {
	display: none;
}

.tab {
	display: flex;
	align-items: center;
	justify-content: center;
	height: 20px;
	width: 87px;
	font-size: 0.7rem;
	color: black;
	font-weight: 500;
	border-radius: 99px;
	cursor: pointer;
	transition: color 0.15s ease-in;
}

.tabs input[type='radio']:checked + label {
	color: #03348fff;
}

.tabs input[id='linear']:checked ~ .glider {
	transform: translateX(0);
}

.tabs input[id='radial']:checked ~ .glider {
	transform: translateX(95%);
}

.tabs input[id='conic']:checked ~ .glider {
	transform: translateX(195%);
}

.tabs input[id='static']:checked ~ .glider {
	transform: translateX(0);
}

.tabs input[id='random']:checked ~ .glider {
	transform: translateX(95%);
}

.tabs input[id='gradient']:checked ~ .glider {
	transform: translateX(195%);
}

.tabs input[id='ball-linear']:checked ~ .glider {
	transform: translateX(0);
}

.tabs input[id='ball-radial']:checked ~ .glider {
	transform: translateX(95%);
}

.tabs input[id='ball-conic']:checked ~ .glider {
	transform: translateX(195%);
}

.glider {
	position: absolute;
	display: flex;
	height: 30px;
	width: 84px;
	margin-top: -4px;
	background-color: #bfd3ecff;
	z-index: 1;
	border-radius: 99px;
	transition: 0.25s ease-out;
}

input[type='color'] {
	appearance: none;
	-moz-appearance: none;
	-webkit-appearance: none;
	background: none;
	border: 0;
	cursor: pointer;
	height: 70px;
	width: 70px;
	border-radius: 16px;
}

*:focus {
	border-radius: 0;
	outline: none;
}

::-webkit-color-swatch-wrapper {
	padding: 0;
}

::-webkit-color-swatch {
	border: 0;
	border-radius: 8px;
}

::-moz-color-swatch,
::-moz-focus-inner {
	border: 0;
}

::-moz-focus-inner {
	padding: 0;
}

.number {
	max-width: 250px;
	width: 250px;
	height: 44px;
	background-color: #afafaf2c;
	border-radius: 12px;
	padding: 0 1rem;
	border: 2px solid transparent;
	font-size: 1rem;
	transition: border-color 0.3s cubic-bezier(0.25, 0.01, 0.25, 1) 0s,
		color 0.3s cubic-bezier(0.25, 0.01, 0.25, 1) 0s,
		background 0.2s cubic-bezier(0.25, 0.01, 0.25, 1) 0s;
}

.number:hover,
.number:focus {
	outline: none;
	border-color: #f0f0f0ff;
}

.number:focus {
	color: #e6e6e6c2;
}
.select-wrapper {
	position: relative;
	width: 100%;
}
.select-wrapper::after {
	color: black;
	content: '▾';
	pointer-events: none;
	position: absolute;
	right: 15px;
	top: 50%;
	transform: translateY(-50%);
	font-size: 18px;
}

.select {
	-moz-appearance: none;
	-webkit-appearance: none;
	appearance: none;
	background: rgb(63, 63, 63);
	border: 1px solid #ccc;
	border-radius: 10px;
	cursor: pointer;
	padding: 10px;
	width: 100%;
	font-size: 16px;
}

.select:focus {
	outline: none;
	border-color: #185ee0;
}

.select::-ms-expand {
	display: none;
}

option:disabled {
	color: gray;
}
.background-menu-close {
	position: absolute;
	width: 100vw;
	height: 100vh;
}
</style>
