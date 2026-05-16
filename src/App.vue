<template>
  <div class="app">
    <div class="phone">

      <!-- 顶部状态栏 -->
      <div class="top">
        <span>9:41</span>
        <span>RideGo 🚗</span>
      </div>

      <!-- 1. 使用功能界面 -->
      <section v-if="screen === 'booking'" class="screen">
        <div class="map">
          <div class="road road-one"></div>
          <div class="road road-two"></div>
          <div class="car">🚗</div>
          <div class="location-dot"></div>
        </div>

        <div class="panel">
          <h1>Where to?</h1>
          <p class="sub">Book a ride in minutes</p>

          <div class="input-card">
            <div class="line green"></div>
            <input v-model="pickup" placeholder="Pickup location" />
          </div>

          <div class="input-card">
            <div class="line red"></div>
            <input v-model="destination" placeholder="Destination" />
          </div>

          <h3>Recommended</h3>

          <div 
            class="ride-option"
            :class="{ active: rideType === 'Economy' }"
            @click="rideType = 'Economy'"
          >
            <span class="car-icon">🚙</span>
            <div>
              <strong>Economy</strong>
              <p>Affordable everyday rides</p>
            </div>
            <b>¥56</b>
          </div>

          <div 
            class="ride-option"
            :class="{ active: rideType === 'Comfort' }"
            @click="rideType = 'Comfort'"
          >
            <span class="car-icon">🚘</span>
            <div>
              <strong>Comfort</strong>
              <p>More space and comfort</p>
            </div>
            <b>¥78</b>
          </div>

          <button @click="requestRide">
            Request Ride →
          </button>
        </div>
      </section>

      <!-- 2. 打车进行中界面 -->
      <section v-if="screen === 'tracking'" class="screen">
        <div class="tracking-map">
          <div class="route"></div>
          <div class="pin start">●</div>
          <div class="pin end">●</div>
          <div class="moving-car">🚗</div>
        </div>

        <div class="panel">
          <h1>Driver is on the way</h1>
          <p class="sub">Arriving in <b>3 min</b></p>

          <div class="driver-card">
            <div class="avatar">👨‍✈️</div>
            <div>
              <strong>Alex Chen ⭐ 4.9</strong>
              <p>Tesla Model 3 · 浙C 88888</p>
            </div>
            <button class="small-btn">☎</button>
          </div>

          <div class="trip-box">
            <div>
              <span>Distance</span>
              <strong>12.6 km</strong>
            </div>
            <div>
              <span>ETA</span>
              <strong>18 min</strong>
            </div>
            <div>
              <span>Fare</span>
              <strong>¥56</strong>
            </div>
          </div>

          <button class="danger" @click="completeRide">
            End Ride
          </button>
        </div>
      </section>

      <!-- 3. 结束界面：支付 + 评价 -->
      <section v-if="screen === 'complete'" class="screen complete">
        <div class="success">
          <div class="check">✓</div>
          <h1>Trip Completed</h1>
          <p>Thank you for riding with us!</p>
        </div>

        <div class="panel">
          <div class="fare-card">
            <h3>Fare Details</h3>
            <p><span>Base Fare</span><b>¥35</b></p>
            <p><span>Distance Fee</span><b>¥16</b></p>
            <p><span>Service Fee</span><b>¥5</b></p>
            <hr />
            <p class="total"><span>Total</span><b>¥56</b></p>
          </div>

          <h3>Rate Your Driver</h3>

          <div class="stars">
            <span 
              v-for="n in 5" 
              :key="n"
              @click="rating = n"
              :class="{ selected: n <= rating }"
            >
              ★
            </span>
          </div>

          <textarea v-model="comment" placeholder="Share your experience"></textarea>

          <button @click="submitReview">
            Pay & Submit Rating
          </button>
        </div>
      </section>

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const screen = ref('booking')
const pickup = ref('Wenzhou-Kean University')
const destination = ref('Wenzhou South Railway Station')
const rideType = ref('Economy')
const rating = ref(5)
const comment = ref('')

function requestRide() {
  screen.value = 'tracking'
}

function completeRide() {
  screen.value = 'complete'
}

function submitReview() {
  alert('Payment successful! Rating submitted.')
  screen.value = 'booking'
}
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: linear-gradient(135deg, #dfe9f3 0%, #ffffff 100%);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}

.app {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* 手机外框 */
.phone {
  width: 390px;
  height: 820px;
  background: #f8f9fb;
  border-radius: 42px;
  overflow: hidden;
  box-shadow: 0 30px 80px rgba(0, 0, 0, 0.18);
  position: relative;
  border: 8px solid #111827;
}

.top {
  height: 48px;
  padding: 16px 24px;
  display: flex;
  justify-content: space-between;
  font-weight: 700;
  color: #111827;
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(20px);
  position: relative;
  z-index: 10;
}

.screen {
  height: calc(100% - 48px);
  position: relative;
}

/* 地图背景 */
.map,
.tracking-map {
  height: 330px;
  background: linear-gradient(135deg, #e9eef7, #ffffff);
  position: relative;
  overflow: hidden;
}

.road {
  position: absolute;
  background: white;
  border-radius: 999px;
  box-shadow: 0 0 0 1px #e5e7eb;
}

.road-one {
  width: 520px;
  height: 28px;
  top: 120px;
  left: -70px;
  transform: rotate(-28deg);
}

.road-two {
  width: 460px;
  height: 24px;
  top: 190px;
  left: -30px;
  transform: rotate(22deg);
}

.car {
  position: absolute;
  top: 138px;
  left: 210px;
  font-size: 28px;
  transform: rotate(-25deg);
}

.location-dot {
  position: absolute;
  top: 160px;
  left: 165px;
  width: 22px;
  height: 22px;
  background: #1a73e8;
  border: 5px solid white;
  border-radius: 50%;
  box-shadow: 0 0 0 18px rgba(26, 115, 232, 0.15);
}

/* 下方面板 */
.panel {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(255,255,255,0.88);
  backdrop-filter: blur(18px);
  padding: 26px;
  border-radius: 32px 32px 0 0;
  box-shadow: 0 -10px 35px rgba(0, 0, 0, 0.08);
}

h1 {
  margin: 0;
  font-size: 28px;
    color: #111827;
  font-weight: 800;
}

.sub {
  color: #4b5563;
  font-weight: 500;
  margin-top: 6px;
  margin-bottom: 20px;
}

.input-card {
  display: flex;
  align-items: center;
  gap: 12px;
  background: white;
  padding: 14px;
  border-radius: 18px;
  margin-bottom: 12px;
  border: 1px solid #edf0f5;
  box-shadow: 0 4px 12px rgba(0,0,0,0.04);
  transition: all 0.25s ease;
}

.input-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 25px rgba(0,0,0,0.08);
}

.line {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}

.green {
  background: #22c55e;
}

.red {
  background: #ef4444;
}

input,
textarea {
  border: none;
  outline: none;
  background: transparent;
  width: 100%;
  font-size: 15px;
  color: #111827;
  font-weight: 600;
}

h3 {
  color: #111827;
  font-size: 20px;
  margin-bottom: 14px;
}

textarea {
  height: 90px;
  background: #f8fafc;
  border: 1px solid #edf0f5;
  border-radius: 18px;
  padding: 14px;
  resize: none;
}

.ride-option {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 14px;
  border-radius: 20px;
  background: #f8fafc;
  margin-bottom: 12px;
  border: 2px solid transparent;
  cursor: pointer;
}

.ride-option.active {
  border-color: #1a73e8;
  background: #eef5ff;
}

.ride-option p {
  margin: 4px 0 0;
  color: #4b5563;
  font-size: 13px;
}

.ride-option strong {
  color: #111827;
  font-size: 18px;
}

.ride-option b {
  color: #111827;
  font-size: 20px;
}

.car-icon {
  font-size: 30px;
}

.ride-option b {
  margin-left: auto;
}

button {
  width: 100%;
  height: 54px;
  border: none;
  border-radius: 18px;
  background: linear-gradient(135deg, #111827, #1e3a8a);
  color: white;
  font-size: 16px;
  font-weight: 700;
  cursor: pointer;
  margin-top: 14px;
}

button:hover {
  transform: scale(0.98);
}

.danger {
  background: #fee2e2;
  color: #dc2626;
}

/* tracking */
.tracking-map {
  height: 420px;
}

.route {
  position: absolute;
  width: 6px;
  height: 260px;
  background: #1a73e8;
  top: 70px;
  left: 190px;
  border-radius: 999px;
  transform: rotate(-20deg);
}

.pin {
  position: absolute;
  font-size: 26px;
}

.start {
  top: 62px;
  left: 170px;
  color: #111827;
}

.end {
  top: 310px;
  left: 245px;
  color: #1a73e8;
}

.moving-car {
  position: absolute;
  top: 210px;
  left: 210px;
  font-size: 32px;
  transform: rotate(70deg);
}

.driver-card {
  display: flex;
  align-items: center;
  gap: 14px;
  background: #f8fafc;
  padding: 16px;
  border-radius: 22px;
  margin-bottom: 16px;
}

.avatar {
  font-size: 36px;
}

.driver-card p {
  margin: 4px 0 0;
  color: #6b7280;
}

.small-btn {
  width: 44px;
  height: 44px;
  margin: 0 0 0 auto;
  border-radius: 50%;
  background: white;
  color: #111827;
  box-shadow: 0 6px 18px rgba(0,0,0,0.08);
}

.trip-box {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  background: #f8fafc;
  border-radius: 20px;
  overflow: hidden;
}

.trip-box div {
  padding: 16px 8px;
  text-align: center;
  border-right: 1px solid #e5e7eb;
}

.trip-box div:last-child {
  border-right: none;
}

.trip-box span {
  display: block;
  color: #6b7280;
  font-size: 12px;
  margin-bottom: 6px;
}

/* complete */
.complete {
  background: linear-gradient(180deg, #dbeafe 0%, #f8f9fb 45%);
}

.success {
  text-align: center;
  padding-top: 70px;
}

.check {
  width: 92px;
  height: 92px;
  background: #22c55e;
  color: white;
  font-size: 58px;
  line-height: 92px;
  border-radius: 50%;
  margin: 0 auto 24px;
  box-shadow: 0 15px 35px rgba(34,197,94,0.35);
}

.success p {
  color: #6b7280;
}

.fare-card {
  background: #f8fafc;
  border-radius: 22px;
  padding: 18px;
  margin-bottom: 18px;
}

.fare-card p {
  display: flex;
  justify-content: space-between;
  color: #4b5563;
}

.fare-card .total {
  font-size: 20px;
  color: #111827;
}

.stars {
  font-size: 34px;
  margin-bottom: 16px;
}

.stars span {
  color: #d1d5db;
  cursor: pointer;
}

.stars span.selected {
  color: #facc15;
}
</style>