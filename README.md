import React, { useState, useRef } from 'react';
import { ChevronRight, Download, RotateCcw, Sparkles, Heart, Users } from 'lucide-react';

const WreathRibbonSimulator = () => {
  const [currentStep, setCurrentStep] = useState(1);
  const [formData, setFormData] = useState({
    wreathType: '',
    relationship: '',
    senderName: '',
    religion: ''
  });
  const [recommendations, setRecommendations] = useState([]);
  const [selectedMessage, setSelectedMessage] = useState('');
  const canvasRef = useRef(null);

  // 문구 추천 로직
  const generateRecommendations = () => {
    const { wreathType, relationship, religion } = formData;
    
    const templates = {
      funeral: {
        buddhist: [
          { text: "명복을 빕니다", reason: "불교 전통에 맞는 정중한 표현" },
          { text: "부처님 품에 안식하소서", reason: "불교적 내세관을 반영한 위로" },
          { text: "극락왕생하소서", reason: "불교 특유의 내세 기원 표현" }
        ],
        christian: [
          { text: "하나님의 품에 안식하소서", reason: "기독교적 위로와 평안 기원" },
          { text: "주님의 사랑 안에서", reason: "기독교적 사랑과 은혜 표현" },
          { text: "영원한 안식을 빕니다", reason: "기독교적 영생 개념 반영" }
        ],
        catholic: [
          { text: "주님의 품에 안식하소서", reason: "천주교 전통적 위로 표현" },
          { text: "영원한 평화를 빕니다", reason: "천주교적 평화와 안식 기원" },
          { text: "성모님의 도우심으로", reason: "천주교 특유의 성모 신심 반영" }
        ],
        none: [
          { text: "삼가 고인의 명복을 빕니다", reason: "종교 무관한 정중한 조문" },
          { text: "영원한 안식을 빕니다", reason: "보편적 위로 표현" },
          { text: "고인의 평안을 기원합니다", reason: "정중하고 따뜻한 마음 전달" }
        ]
      },
      celebration: {
        buddhist: [
          { text: "부처님의 가피로 번영하소서", reason: "불교적 축복과 번영 기원" },
          { text: "만사형통하소서", reason: "전통적 축하 표현" },
          { text: "법연무변하소서", reason: "불교적 교화와 발전 기원" }
        ],
        christian: [
          { text: "하나님의 축복이 함께하소서", reason: "기독교적 축복 기원" },
          { text: "주님의 은혜가 충만하소서", reason: "기독교적 은혜와 사랑 표현" },
          { text: "하나님의 영광이 함께하소서", reason: "기독교적 영광과 축복" }
        ],
        catholic: [
          { text: "주님의 축복이 함께하소서", reason: "천주교적 축복 기원" },
          { text: "성모님의 보호하심으로", reason: "천주교 성모 신심 표현" },
          { text: "하느님의 은총이 충만하소서", reason: "천주교적 은총 기원" }
        ],
        none: [
          { text: "축하드립니다", reason: "간결하고 따뜻한 축하" },
          { text: "번영과 발전을 기원합니다", reason: "사업적 성공 기원" },
          { text: "무궁한 발전을 빕니다", reason: "지속적인 성장 기원" }
        ]
      }
    };

    const religionKey = religion === '불교' ? 'buddhist' : 
                      religion === '기독교' ? 'christian' : 
                      religion === '천주교' ? 'catholic' : 'none';
    
    const typeKey = wreathType === 'funeral' ? 'funeral' : 'celebration';
    
    return templates[typeKey][religionKey] || templates[typeKey]['none'];
  };

  const handleNext = () => {
    if (currentStep === 2) {
      const recs = generateRecommendations();
      setRecommendations(recs);
    }
    setCurrentStep(currentStep + 1);
  };

  const handleSelectMessage = (message) => {
    setSelectedMessage(message);
    setCurrentStep(4);
  };

  const simulateRibbon = () => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    
    // 캔버스 크기 설정
    canvas.width = 500;
    canvas.height = 600;
    
    // Helper function to get dynamic font size based on text length
    const getDynamicFontSize = (text, maxWidth = 180) => {
      const baseSize = 16;
      if (text.length <= 8) return `bold ${baseSize}px Arial, sans-serif`;
      if (text.length <= 12) return `bold ${Math.max(14, baseSize - 2)}px Arial, sans-serif`;
      if (text.length <= 16) return `bold ${Math.max(12, baseSize - 4)}px Arial, sans-serif`;
      return `bold ${Math.max(10, baseSize - 6)}px Arial, sans-serif`;
    };
    
    // 화환 종류에 따른 기본 이미지 생성
    const wreathImage = new Image();
    wreathImage.crossOrigin = 'anonymous';
    
    wreathImage.onload = () => {
      // 캔버스 클리어
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      
      // 화환 이미지 그리기
      ctx.drawImage(wreathImage, 0, 0, canvas.width, canvas.height);
      
      // 리본 배경 그리기
      const ribbonHeight = 35;
      const ribbonY = 60;
      
      // 왼쪽 리본 배경
      ctx.fillStyle = 'rgba(236, 72, 153, 0.95)';
      ctx.fillRect(20, ribbonY, 200, ribbonHeight);
      
      // 오른쪽 리본 배경  
      ctx.fillStyle = 'rgba(236, 72, 153, 0.95)';
      ctx.fillRect(280, ribbonY, 200, ribbonHeight);
      
      // 리본 테두리
      ctx.strokeStyle = '#be185d';
      ctx.lineWidth = 2;
      ctx.strokeRect(20, ribbonY, 200, ribbonHeight);
      ctx.strokeRect(280, ribbonY, 200, ribbonHeight);
      
      // 텍스트 공통 스타일 설정
      ctx.fillStyle = '#ffffff';
      ctx.textAlign = 'center';
      ctx.shadowColor = 'rgba(0, 0, 0, 0.5)';
      ctx.shadowBlur = 2;
      ctx.shadowOffsetX = 1;
      ctx.shadowOffsetY = 1;
      
      // 왼쪽 리본 텍스트 (보내는 사람)
      const leftText = formData.senderName || '';
      const leftX = 120;
      const leftY = ribbonY + 22;
      
      ctx.font = getDynamicFontSize(leftText);
      ctx.fillText(leftText, leftX, leftY);
      
      // 오른쪽 리본 텍스트 (추천 문구)
      const rightText = selectedMessage || '';
      const rightX = 380;
      const rightY = ribbonY + 22;
      
      ctx.font = getDynamicFontSize(rightText);
      ctx.fillText(rightText, rightX, rightY);
      
      // 그림자 효과 제거
      ctx.shadowColor = 'transparent';
      ctx.shadowBlur = 0;
      ctx.shadowOffsetX = 0;
      ctx.shadowOffsetY = 0;
      
      // 리본 장식
      ctx.fillStyle = '#ec4899';
      // 왼쪽 리본 장식
      ctx.beginPath();
      ctx.moveTo(20, ribbonY + ribbonHeight);
      ctx.lineTo(35, ribbonY + ribbonHeight + 15);
      ctx.lineTo(50, ribbonY + ribbonHeight);
      ctx.fill();
      
      ctx.beginPath();
      ctx.moveTo(170, ribbonY + ribbonHeight);
      ctx.lineTo(185, ribbonY + ribbonHeight + 15);
      ctx.lineTo(200, ribbonY + ribbonHeight);
      ctx.fill();
      
      // 오른쪽 리본 장식
      ctx.beginPath();
      ctx.moveTo(280, ribbonY + ribbonHeight);
      ctx.lineTo(295, ribbonY + ribbonHeight + 15);
      ctx.lineTo(310, ribbonY + ribbonHeight);
      ctx.fill();
      
      ctx.beginPath();
      ctx.moveTo(450, ribbonY + ribbonHeight);
      ctx.lineTo(465, ribbonY + ribbonHeight + 15);
      ctx.lineTo(480, ribbonY + ribbonHeight);
      ctx.fill();
    };
    
    // Helper function to safely encode SVG with Unicode characters
    const encodeSVG = (svgString) => {
      try {
        return 'data:image/svg+xml;base64,' + btoa(unescape(encodeURIComponent(svgString)));
      } catch (error) {
        console.error('Error encoding SVG:', error);
        // Fallback to URL encoding if base64 fails
        return 'data:image/svg+xml;charset=utf-8,' + encodeURIComponent(svgString);
      }
    };
    
    // 화환 종류에 따른 이미지 URL 설정
    if (formData.wreathType === 'funeral') {
      // 부고 화환 - 차분한 색상의 화환
      const funeralSVG = `
        <svg width="500" height="600" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <radialGradient id="bg" cx="50%" cy="40%" r="60%">
              <stop offset="0%" style="stop-color:#f8fafc"/>
              <stop offset="100%" style="stop-color:#e2e8f0"/>
            </radialGradient>
          </defs>
          <rect width="500" height="600" fill="url(#bg)"/>
          
          <!-- 화환 기본 형태 -->
          <ellipse cx="250" cy="300" rx="180" ry="220" fill="#2d5016" stroke="#1a3009" stroke-width="3"/>
          <ellipse cx="250" cy="300" rx="100" ry="130" fill="#f8fafc"/>
          
          <!-- 조문 화환 꽃들 (차분한 색상) -->
          <circle cx="150" cy="180" r="12" fill="#dc2626"/>
          <circle cx="180" cy="160" r="10" fill="#fbbf24"/>
          <circle cx="220" cy="150" r="11" fill="#dc2626"/>
          <circle cx="280" cy="150" r="10" fill="#f59e0b"/>
          <circle cx="320" cy="160" r="12" fill="#dc2626"/>
          <circle cx="350" cy="180" r="11" fill="#fbbf24"/>
          
          <circle cx="120" cy="220" r="11" fill="#dc2626"/>
          <circle cx="380" cy="220" r="12" fill="#f59e0b"/>
          <circle cx="100" cy="280" r="10" fill="#fbbf24"/>
          <circle cx="400" cy="280" r="11" fill="#dc2626"/>
          <circle cx="120" cy="340" r="12" fill="#f59e0b"/>
          <circle cx="380" cy="340" r="10" fill="#fbbf24"/>
          
          <circle cx="150" cy="420" r="11" fill="#dc2626"/>
          <circle cx="180" cy="440" r="12" fill="#f59e0b"/>
          <circle cx="220" cy="450" r="10" fill="#fbbf24"/>
          <circle cx="280" cy="450" r="11" fill="#dc2626"/>
          <circle cx="320" cy="440" r="12" fill="#f59e0b"/>
          <circle cx="350" cy="420" r="10" fill="#fbbf24"/>
          
          <!-- 잎사귀 -->
          <ellipse cx="250" cy="120" rx="8" ry="15" fill="#16a34a" transform="rotate(45 250 120)"/>
          <ellipse cx="250" cy="480" rx="8" ry="15" fill="#16a34a" transform="rotate(-45 250 480)"/>
          <ellipse cx="80" cy="300" rx="8" ry="15" fill="#16a34a"/>
          <ellipse cx="420" cy="300" rx="8" ry="15" fill="#16a34a"/>
        </svg>
      `;
      wreathImage.src = encodeSVG(funeralSVG);
    } else {
      // 축하 화환 - 밝고 화려한 색상의 화환
      const celebrationSVG = `
        <svg width="500" height="600" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <radialGradient id="bg" cx="50%" cy="40%" r="60%">
              <stop offset="0%" style="stop-color:#fef3c7"/>
              <stop offset="100%" style="stop-color:#f3e8ff"/>
            </radialGradient>
          </defs>
          <rect width="500" height="600" fill="url(#bg)"/>
          
          <!-- 화환 기본 형태 -->
          <ellipse cx="250" cy="300" rx="180" ry="220" fill="#15803d" stroke="#14532d" stroke-width="3"/>
          <ellipse cx="250" cy="300" rx="100" ry="130" fill="#fefce8"/>
          
          <!-- 축하 화환 꽃들 (화려한 색상) -->
          <circle cx="150" cy="180" r="12" fill="#ef4444"/>
          <circle cx="180" cy="160" r="10" fill="#f97316"/>
          <circle cx="220" cy="150" r="11" fill="#eab308"/>
          <circle cx="280" cy="150" r="10" fill="#ec4899"/>
          <circle cx="320" cy="160" r="12" fill="#8b5cf6"/>
          <circle cx="350" cy="180" r="11" fill="#06b6d4"/>
          
          <circle cx="120" cy="220" r="11" fill="#10b981"/>
          <circle cx="380" cy="220" r="12" fill="#f59e0b"/>
          <circle cx="100" cy="280" r="10" fill="#ef4444"/>
          <circle cx="400" cy="280" r="11" fill="#ec4899"/>
          <circle cx="120" cy="340" r="12" fill="#8b5cf6"/>
          <circle cx="380" cy="340" r="10" fill="#06b6d4"/>
          
          <circle cx="150" cy="420" r="11" fill="#10b981"/>
          <circle cx="180" cy="440" r="12" fill="#f97316"/>
          <circle cx="220" cy="450" r="10" fill="#eab308"/>
          <circle cx="280" cy="450" r="11" fill="#ef4444"/>
          <circle cx="320" cy="440" r="12" fill="#ec4899"/>
          <circle cx="350" cy="420" r="10" fill="#8b5cf6"/>
          
          <!-- 더 많은 장식 꽃들 -->
          <circle cx="200" cy="190" r="8" fill="#fbbf24"/>
          <circle cx="300" cy="190" r="8" fill="#f472b6"/>
          <circle cx="170" cy="380" r="8" fill="#34d399"/>
          <circle cx="330" cy="380" r="8" fill="#60a5fa"/>
          
          <!-- 잎사귀 -->
          <ellipse cx="250" cy="120" rx="8" ry="15" fill="#22c55e" transform="rotate(45 250 120)"/>
          <ellipse cx="250" cy="480" rx="8" ry="15" fill="#22c55e" transform="rotate(-45 250 480)"/>
          <ellipse cx="80" cy="300" rx="8" ry="15" fill="#22c55e"/>
          <ellipse cx="420" cy="300" rx="8" ry="15" fill="#22c55e"/>
        </svg>
      `;
      wreathImage.src = encodeSVG(celebrationSVG);
    }
  };

  React.useEffect(() => {
    if (currentStep === 4 && canvasRef.current) {
      simulateRibbon();
    }
  }, [currentStep, selectedMessage, formData.senderName]);

  const downloadImage = () => {
    const canvas = canvasRef.current;
    const link = document.createElement('a');
    link.download = '화환_시뮬레이션.png';
    link.href = canvas.toDataURL();
    link.click();
  };

  const resetForm = () => {
    setCurrentStep(1);
    setFormData({
      wreathType: '',
      relationship: '',
      senderName: '',
      religion: ''
    });
    setRecommendations([]);
    setSelectedMessage('');
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-purple-50 via-pink-50 to-blue-50">
      {/* 헤더 */}
      <div className="bg-white/80 backdrop-blur-sm border-b border-purple-100 sticky top-0 z-50">
        <div className="max-w-4xl mx-auto px-4 py-4">
          <div className="flex items-center justify-between">
            <div className="flex items-center space-x-3">
              <div className="bg-gradient-to-r from-purple-600 to-pink-600 p-2 rounded-xl">
                <Sparkles className="w-6 h-6 text-white" />
              </div>
              <div>
                <h1 className="text-2xl font-bold bg-gradient-to-r from-purple-600 to-pink-600 bg-clip-text text-transparent">
                  화환 리본 문구 추천
                </h1>
                <p className="text-sm text-gray-600">AI가 추천하는 맞춤형 리본 문구</p>
              </div>
            </div>
            <div className="flex items-center space-x-2">
              <div className="flex space-x-1">
                {[1, 2, 3, 4].map((step) => (
                  <div
                    key={step}
                    className={`w-3 h-3 rounded-full transition-all duration-300 ${
                      step <= currentStep
                        ? 'bg-gradient-to-r from-purple-500 to-pink-500 shadow-sm'
                        : 'bg-gray-200'
                    }`}
                  />
                ))}
              </div>
            </div>
          </div>
        </div>
      </div>

      <div className="max-w-4xl mx-auto px-4 py-8">
        {/* Step 1: 화환 종류 선택 */}
        {currentStep === 1 && (
          <div className="space-y-8">
            <div className="text-center">
              <h2 className="text-3xl font-bold text-gray-800 mb-4">
                화환 종류를 선택해주세요
              </h2>
              <p className="text-lg text-gray-600">
                상황에 맞는 적절한 문구를 추천해드립니다
              </p>
            </div>
            
            <div className="grid md:grid-cols-2 gap-6 max-w-2xl mx-auto">
              <button
                onClick={() => setFormData({...formData, wreathType: 'funeral'})}
                className={`group relative overflow-hidden rounded-2xl p-8 transition-all duration-300 ${
                  formData.wreathType === 'funeral'
                    ? 'bg-gradient-to-br from-gray-100 to-gray-200 ring-4 ring-gray-300 shadow-2xl scale-105'
                    : 'bg-white hover:bg-gray-50 shadow-lg hover:shadow-xl hover:scale-102'
                }`}
              >
                <div className="relative z-10">
                  <div className="w-16 h-16 bg-gradient-to-br from-gray-500 to-gray-700 rounded-full flex items-center justify-center mb-4 mx-auto">
                    <Heart className="w-8 h-8 text-white" />
                  </div>
                  <h3 className="text-xl font-bold text-gray-800 mb-2">부고 화환</h3>
                  <p className="text-gray-600">조문 및 위로를 위한 화환</p>
                </div>
                <div className="absolute inset-0 bg-gradient-to-br from-gray-400/20 to-gray-600/20 opacity-0 group-hover:opacity-100 transition-opacity duration-300" />
              </button>

              <button
                onClick={() => setFormData({...formData, wreathType: 'celebration'})}
                className={`group relative overflow-hidden rounded-2xl p-8 transition-all duration-300 ${
                  formData.wreathType === 'celebration'
                    ? 'bg-gradient-to-br from-pink-100 to-purple-100 ring-4 ring-pink-300 shadow-2xl scale-105'
                    : 'bg-white hover:bg-gradient-to-br hover:from-pink-50 hover:to-purple-50 shadow-lg hover:shadow-xl hover:scale-102'
                }`}
              >
                <div className="relative z-10">
                  <div className="w-16 h-16 bg-gradient-to-br from-pink-500 to-purple-600 rounded-full flex items-center justify-center mb-4 mx-auto">
                    <Sparkles className="w-8 h-8 text-white" />
                  </div>
                  <h3 className="text-xl font-bold text-gray-800 mb-2">축하 화환</h3>
                  <p className="text-gray-600">개업, 승진 등 축하를 위한 화환</p>
                </div>
                <div className="absolute inset-0 bg-gradient-to-br from-pink-400/20 to-purple-600/20 opacity-0 group-hover:opacity-100 transition-opacity duration-300" />
              </button>
            </div>

            {formData.wreathType && (
              <div className="flex justify-center pt-4">
                <button
                  onClick={handleNext}
                  className="group bg-gradient-to-r from-purple-600 to-pink-600 text-white px-8 py-4 rounded-full font-semibold hover:from-purple-700 hover:to-pink-700 transition-all duration-300 shadow-lg hover:shadow-xl flex items-center space-x-2"
                >
                  <span>다음 단계</span>
                  <ChevronRight className="w-5 h-5 group-hover:translate-x-1 transition-transform" />
                </button>
              </div>
            )}
          </div>
        )}

        {/* Step 2: 상세 정보 입력 */}
        {currentStep === 2 && (
          <div className="space-y-8">
            <div className="text-center">
              <h2 className="text-3xl font-bold text-gray-800 mb-4">
                상세 정보를 입력해주세요
              </h2>
              <p className="text-lg text-gray-600">
                맞춤형 문구 추천을 위한 정보입니다
              </p>
            </div>

            <div className="max-w-2xl mx-auto bg-white rounded-2xl shadow-xl p-8 space-y-6">
              <div>
                <label className="block text-sm font-semibold text-gray-700 mb-3">
                  받는 사람과의 관계
                </label>
                <select
                  value={formData.relationship}
                  onChange={(e) => setFormData({...formData, relationship: e.target.value})}
                  className="w-full px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-purple-500 focus:border-transparent transition-all duration-200"
                >
                  <option value="">선택해주세요</option>
                  <option value="상사">상사</option>
                  <option value="동료">동료</option>
                  <option value="부하직원">부하직원</option>
                  <option value="친구">친구</option>
                  <option value="가족">가족</option>
                  <option value="고객">고객</option>
                  <option value="거래처">거래처</option>
                  <option value="지인">지인</option>
                </select>
              </div>

              <div>
                <label className="block text-sm font-semibold text-gray-700 mb-3">
                  보내는 사람 (소속 또는 이름)
                </label>
                <input
                  type="text"
                  value={formData.senderName}
                  onChange={(e) => setFormData({...formData, senderName: e.target.value})}
                  placeholder="예: 홍길동, ㈜테크컴퍼니, 김철수 외 임직원 일동"
                  className="w-full px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-purple-500 focus:border-transparent transition-all duration-200"
                />
              </div>

              <div>
                <label className="block text-sm font-semibold text-gray-700 mb-3">
                  받는 사람의 종교
                </label>
                <select
                  value={formData.religion}
                  onChange={(e) => setFormData({...formData, religion: e.target.value})}
                  className="w-full px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-purple-500 focus:border-transparent transition-all duration-200"
                >
                  <option value="">선택해주세요</option>
                  <option value="불교">불교</option>
                  <option value="기독교">기독교</option>
                  <option value="천주교">천주교</option>
                  <option value="무교">무교/기타</option>
                </select>
              </div>
            </div>

            {formData.relationship && formData.senderName && formData.religion && (
              <div className="flex justify-center pt-4">
                <button
                  onClick={handleNext}
                  className="group bg-gradient-to-r from-purple-600 to-pink-600 text-white px-8 py-4 rounded-full font-semibold hover:from-purple-700 hover:to-pink-700 transition-all duration-300 shadow-lg hover:shadow-xl flex items-center space-x-2"
                >
                  <span>문구 추천받기</span>
                  <ChevronRight className="w-5 h-5 group-hover:translate-x-1 transition-transform" />
                </button>
              </div>
            )}
          </div>
        )}

        {/* Step 3: 문구 추천 */}
        {currentStep === 3 && (
          <div className="space-y-8">
            <div className="text-center">
              <h2 className="text-3xl font-bold text-gray-800 mb-4">
                추천 문구를 선택해주세요
              </h2>
              <p className="text-lg text-gray-600">
                상황에 맞는 문구를 추천해드렸습니다
              </p>
            </div>

            <div className="grid gap-6 max-w-3xl mx-auto">
              {recommendations.map((rec, index) => (
                <button
                  key={index}
                  onClick={() => handleSelectMessage(rec.text)}
                  className="group bg-white rounded-2xl p-6 shadow-lg hover:shadow-xl transition-all duration-300 hover:scale-102 text-left border-2 border-transparent hover:border-purple-200"
                >
                  <div className="flex items-start space-x-4">
                    <div className="w-12 h-12 bg-gradient-to-br from-purple-500 to-pink-500 rounded-full flex items-center justify-center flex-shrink-0">
                      <span className="text-white font-bold text-lg">{index + 1}</span>
                    </div>
                    <div className="flex-1">
                      <h3 className="text-xl font-bold text-gray-800 mb-2 group-hover:text-purple-600 transition-colors">
                        "{rec.text}"
                      </h3>
                      <p className="text-gray-600 text-sm leading-relaxed">
                        {rec.reason}
                      </p>
                    </div>
                    <ChevronRight className="w-5 h-5 text-gray-400 group-hover:text-purple-500 group-hover:translate-x-1 transition-all" />
                  </div>
                </button>
              ))}
            </div>
          </div>
        )}

        {/* Step 4: 시뮬레이션 결과 */}
        {currentStep === 4 && (
          <div className="space-y-8">
            <div className="text-center">
              <h2 className="text-3xl font-bold text-gray-800 mb-4">
                화환 시뮬레이션 결과
              </h2>
              <p className="text-lg text-gray-600">
                실제 화환에 적용된 모습을 확인해보세요
              </p>
            </div>

            <div className="max-w-2xl mx-auto bg-white rounded-2xl shadow-2xl p-8">
              <div className="text-center mb-6">
                <canvas
                  ref={canvasRef}
                  className="max-w-full h-auto rounded-xl shadow-lg mx-auto"
                  style={{ maxHeight: '600px' }}
                />
              </div>

              <div className="bg-gradient-to-r from-purple-50 to-pink-50 rounded-xl p-6 mb-6">
                <div className="grid md:grid-cols-2 gap-4 text-sm">
                  <div>
                    <span className="font-semibold text-gray-700">왼쪽 리본:</span>
                    <div className="bg-white rounded-lg p-3 mt-2 shadow-sm">
                      {formData.senderName}
                    </div>
                  </div>
                  <div>
                    <span className="font-semibold text-gray-700">오른쪽 리본:</span>
                    <div className="bg-white rounded-lg p-3 mt-2 shadow-sm">
                      {selectedMessage}
                    </div>
                  </div>
                </div>
              </div>

              <div className="flex flex-col sm:flex-row gap-4 justify-center">
                <button
                  onClick={downloadImage}
                  className="flex items-center justify-center space-x-2 bg-gradient-to-r from-green-500 to-emerald-500 text-white px-6 py-3 rounded-full font-semibold hover:from-green-600 hover:to-emerald-600 transition-all duration-300 shadow-lg hover:shadow-xl"
                >
                  <Download className="w-5 h-5" />
                  <span>이미지 다운로드</span>
                </button>
                <button
                  onClick={resetForm}
                  className="flex items-center justify-center space-x-2 bg-gradient-to-r from-gray-500 to-gray-600 text-white px-6 py-3 rounded-full font-semibold hover:from-gray-600 hover:to-gray-700 transition-all duration-300 shadow-lg hover:shadow-xl"
                >
                  <RotateCcw className="w-5 h-5" />
                  <span>다시 시작</span>
                </button>
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
};

export default WreathRibbonSimulator;
