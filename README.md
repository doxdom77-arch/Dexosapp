import React, { useState } from 'react';
import { 
  StyleSheet, 
  Text, 
  View, 
  TouchableOpacity, 
  ScrollView, 
  SafeAreaView, 
  Modal, 
  TextInput, 
  Alert,
  Linking
} from 'react-native';

// รายชื่อสถานีอวกาศทั้ง 10 แห่ง
const STATIONS = [
  { id: '1', name: 'DEXOS Prime Mother Ship', theme: 'Future Cyber Neon', color: '#00F0FF', ambient: 'rgba(0, 240, 255, 0.25)', desc: 'สถานีหลักความเร็วสูง สไตล์อนาคตไซเบอร์' },
  { id: '2', name: 'Aethelgard Fantasy Station', theme: 'Fantasy Glassmorphism', color: '#FFB7FF', ambient: 'rgba(255, 183, 255, 0.25)', desc: 'ดินแดนแห่งจินตนาการและกระจกแก้วเวทมนตร์' },
  { id: '3', name: 'Nebula Flux Multiverse', theme: 'Aurora Gradients', color: '#A020F0', ambient: 'rgba(160, 32, 240, 0.25)', desc: 'ห้วงจักรวาลแห่งสีสันที่ไหลลื่นตามการใช้งาน' },
  { id: '4', name: 'Veridian Biome Station', theme: 'Eco-Sanctuary', color: '#2ECC71', ambient: 'rgba(46, 204, 113, 0.25)', desc: 'สถานีธรรมชาติบำบัดและรักษาสิ่งแวดล้อม' },
  { id: '5', name: 'Aeterna Pure White Zenith', theme: 'Minimalist Focus', color: '#333333', ambient: 'rgba(255, 255, 255, 0.4)', desc: 'พื้นที่ขาวสะอาดบริสุทธิ์เพื่อสมาธิขั้นสูงสุด' },
  { id: '6', name: 'Sakura Serenity Garden', theme: 'Soft Petals', color: '#FFC0CB', ambient: 'rgba(255, 192, 203, 0.25)', desc: 'สวนซากุระแห่งความสงบและละมุนละไม' },
  { id: '7', name: 'Obsidian Crystal Core', theme: 'Underground Vault', color: '#34495E', ambient: 'rgba(52, 73, 94, 0.4)', desc: 'ห้องนิรภัยใต้ดินเข้ารหัสควอนตัมปลอดภัยสูงสุด' },
  { id: '8', name: 'Chronos Time Museum', theme: 'Heritage Bronze', color: '#D35400', ambient: 'rgba(211, 84, 0, 0.25)', desc: 'พิพิธภัณฑ์แห่งกาลเวลาและมรดกทางปัญญา' },
  { id: '9', name: 'Childhood Cartoon Oasis', theme: 'Vibrant Playful', color: '#F1C40F', ambient: 'rgba(241, 196, 15, 0.25)', desc: 'โอเอซิสแห่งความสดใสและปลอดภัยสำหรับเยาวชน' },
  { id: '10', name: 'Olympus Kinetic Arena', theme: 'Sports & Vitality Hub', color: '#E74C3C', ambient: 'rgba(231, 76, 60, 0.25)', desc: 'ศูนย์รวมข่าวกีฬาและครูฝึก AI โฮโลแกรมส่วนตัว' },
];

export default function App() {
  const [currentStation, setCurrentStation] = useState(STATIONS[0]);
  const [activeTab, setActiveTab] = useState('feed'); // 'feed' | 'sports' | 'tools' | 'value'
  const [supportModalVisible, setSupportModalVisible] = useState(false);
  const [customAmount, setCustomAmount] = useState('');
  const [isLightingActive, setIsLightingActive] = useState(true);

  const handleValueContribution = (amount) => {
    Alert.alert(
      'ขอบคุณจากใจจริง 💖', 
      `คุณได้สนับสนุนคุณค่าด้วยจำนวนเงิน/เครดิต ${amount} เรียบร้อยแล้ว ระบบได้ส่งความขอบคุณตรงถึงครีเอเตอร์ด้วยความโปร่งใส 100%`
    );
    setSupportModalVisible(false);
    setCustomAmount('');
  };

  const openExternalLink = (url) => {
    Linking.openURL(url).catch((err) => console.error("ไม่สามารถเปิดลิงก์ได้:", err));
  };

  return (
    <SafeAreaView style={[styles.container, { backgroundColor: currentStation.id === '5' ? '#F5F5F5' : '#0B0F19' }]}>
      
      {/* ส่วนหัวแอป */}
      <View style={styles.header}>
        <Text style={styles.motto}>"สร้างสรรค์เทคโนโลยี เพื่อคุณภาพชีวิตที่ดีของประชาชนทั่วโลก"</Text>
        <Text style={[styles.stationTitle, { color: currentStation.color }]}>✨ {currentStation.name}</Text>
        <Text style={styles.stationDesc}>{currentStation.desc}</Text>
      </View>

      {/* พื้นที่แสดงผลหลัก (Holographic Viewport) */}
      <View style={[
        styles.viewport, 
        { 
          borderColor: currentStation.color,
          backgroundColor: isLightingActive ? currentStation.ambient : 'rgba(255, 255, 255, 0.02)',
          shadowColor: currentStation.color,
          shadowOpacity: isLightingActive ? 0.8 : 0,
          shadowRadius: 15,
          elevation: isLightingActive ? 10 : 0
        }
      ]}>
        {activeTab === 'feed' && (
          <ScrollView contentContainerStyle={styles.tabContent} showsVerticalScrollIndicator={false}>
            <Text style={[styles.viewportTitle, { color: currentStation.color }]}>📜 Sovereign Merit-Based Feed</Text>
            <Text style={styles.bodyText}>• ปราศจากฟีดขยะและโฆษณาคั่นพิษภัย</Text>
            <Text style={styles.bodyText}>• จัดอันดับด้วยคุณค่า (Quality over Quantity)</Text>
            <Text style={styles.bodyText}>• ปลอดภัยด้วย Silent AI สะกิดเตือนความรู้สึกละมุนใจ</Text>
          </ScrollView>
        )}

        {activeTab === 'sports' && (
          <ScrollView contentContainerStyle={styles.tabContent} showsVerticalScrollIndicator={false}>
            <Text style={[styles.viewportTitle, { color: currentStation.color }]}>🏋️‍♂️ Olympus Kinetic Arena</Text>
            <Text style={styles.bodyText}>• ข่าวกีฬาและตารางแข่งขันสะอาดไร้คลิกเบท</Text>
            <Text style={styles.bodyText}>• เลือกครูฝึก AI โฮโลแกรม (ชาย/หญิง) สมจริง 100%</Text>
            <Text style={styles.bodyText}>• ระบบวิเคราะห์ท่าทางป้องกันการบาดเจ็บแบบเรียลไทม์</Text>
          </ScrollView>
        )}

        {activeTab === 'tools' && (
          <ScrollView contentContainerStyle={styles.tabContent} showsVerticalScrollIndicator={false}>
            <Text style={[styles.viewportTitle, { color: currentStation.color }]}>🛠️ DEXOS Core & Tool Links</Text>
            
            {/* ปุ่ม Writing Code on Your Mobile */}
            <TouchableOpacity style={[styles.toolCard, { borderColor: '#2ECC71' }]} onPress={() => openExternalLink('https://expo.dev')}>
              <Text style={styles.toolEmoji}>💻</Text>
              <View style={styles.toolTextWrapper}>
                <Text style={styles.toolTitle}>Writing Code on Your Mobile</Text>
                <Text style={styles.toolSubtitle}>เปิดเครื่องมือแก้ไขโค้ดและพัฒนาแอปผ่านมือถือ</Text>
              </View>
            </TouchableOpacity>

            {/* ปุ่ม Animation Engine */}
            <TouchableOpacity style={[styles.toolCard, { borderColor: '#00F0FF' }]} onPress={() => openExternalLink('https://reactnative.dev/docs/animations')}>
              <Text style={styles.toolEmoji}>✨</Text>
              <View style={styles.toolTextWrapper}>
                <Text style={styles.toolTitle}>Dynamic Animation Engine</Text>
                <Text style={styles.toolSubtitle}>ควบคุมเอฟเฟกต์แอนิเมชันลื่นไหลสไตล์ไซเบอร์</Text>
              </View>
            </TouchableOpacity>

            {/* ปุ่ม 2D Graphics */}
            <TouchableOpacity style={[styles.toolCard, { borderColor: '#A020F0' }]} onPress={() => openExternalLink('https://shopify.github.io/react-native-skia/')}>
              <Text style={styles.toolEmoji}>🎨</Text>
              <View style={styles.toolTextWrapper}>
                <Text style={styles.toolTitle}>2D Graphics Canvas Engine</Text>
                <Text style={styles.toolSubtitle}>ระบบเรนเดอร์ภาพกราฟิก 2 มิติและแคนวาสอัจฉริยะ</Text>
              </View>
            </TouchableOpacity>

            {/* ปุ่ม Holographic Optics (Hoptics) */}
            <TouchableOpacity style={[styles.toolCard, { borderColor: '#FFB7FF' }]} onPress={() => openExternalLink('https://your-dexos-app.vercel.app/#hoptics')}>
              <Text style={styles.toolEmoji}>🔮</Text>
              <View style={styles.toolTextWrapper}>
                <Text style={styles.toolTitle}>Hoptics Protocol (Holographic Optics)</Text>
                <Text style={styles.toolSubtitle}>ระบบจำลองและควบคุมแสงเลนส์โฮโลแกรมอัจฉริยะ</Text>
              </View>
            </TouchableOpacity>

            {/* ปุ่มควบคุมระบบแสงสี (Lighting Toggle) */}
            <TouchableOpacity style={[styles.ambientToggleBtn, { backgroundColor: currentStation.color }]} onPress={() => setIsLightingActive(!isLightingActive)}>
              <Text style={[styles.ambientToggleText, { color: currentStation.id === '5' ? '#FFF' : '#000' }]}>
                {isLightingActive ? '⚡ ปิดระบบแสงสีออร่า' : '💡 เปิดระบบแสงสีออร่าโฮโลแกรม'}
              </Text>
            </TouchableOpacity>
          </ScrollView>
        )}

        {activeTab === 'value' && (
          <ScrollView contentContainerStyle={styles.tabContent} showsVerticalScrollIndicator={false}>
            <Text style={[styles.viewportTitle, { color: currentStation.color }]}>💎 Pay-What-You-Value Protocol</Text>
            <Text style={styles.bodyText}>เลือกสนับสนุนผลงาน คลาสเรียน หรือบริการตามความพึงพอใจและคุณค่าที่คุณได้รับจริง</Text>
            <TouchableOpacity style={[styles.supportButton, { backgroundColor: currentStation.color }]} onPress={() => setSupportModalVisible(true)}>
              <Text style={styles.supportButtonText}>💖 เลือกสนับสนุนตามความพึงพอใจ</Text>
            </TouchableOpacity>
          </ScrollView>
        )}
      </View>

      {/* แถบเลือกสถานีอวกาศทั้ง 10 แห่ง */}
      <View style={styles.stationSection}>
        <Text style={styles.menuLabel}>🌐 เลือกเปลี่ยนสถานีอวกาศ (1 - 10):</Text>
        <ScrollView horizontal showsHorizontalScrollIndicator={false} style={styles.stationList}>
          {STATIONS.map((station) => (
            <TouchableOpacity
              key={station.id}
              style={[
                styles.stationButton,
                { backgroundColor: currentStation.id === station.id ? station.color : '#1F2937' },
              ]}
              onPress={() => setCurrentStation(station)}
            >
              <Text style={[styles.stationButtonText, { color: currentStation.id === station.id && station.id !== '5' ? '#000' : '#FFF' }]}>
                {station.id}. {station.name.split(' ')[0]}
              </Text>
            </TouchableOpacity>
          ))}
        </ScrollView>
      </View>

      {/* แถบเมนูด้านล่าง (Navigation Bar) */}
      <View style={styles.footer}>
        <TouchableOpacity style={[styles.footerTab, activeTab === 'feed' && styles.activeTab]} onPress={() => setActiveTab('feed')}>
          <Text style={styles.footerText}>📜 Feed</Text>
        </TouchableOpacity>
        <TouchableOpacity style={[styles.footerTab, activeTab === 'sports' && styles.activeTab]} onPress={() => setActiveTab('sports')}>
          <Text style={styles.footerText}>🏋️‍♂️ Arena</Text>
        </TouchableOpacity>
        <TouchableOpacity style={[styles.footerTab, activeTab === 'tools' && styles.activeTab]} onPress={() => setActiveTab('tools')}>
          <Text style={styles.footerText}>🛠️ Tools</Text>
        </TouchableOpacity>
        <TouchableOpacity style={[styles.footerTab, activeTab === 'value' && styles.activeTab]} onPress={() => setActiveTab('value')}>
          <Text style={styles.footerText}>💎 Value</Text>
        </TouchableOpacity>
      </View>

      {/* Modal หน้าต่างสนับสนุน */}
      <Modal animationType="slide" transparent={true} visible={supportModalVisible} onRequestClose={() => setSupportModalVisible(false)}>
        <View style={styles.modalOverlay}>
          <View style={styles.modalContent}>
            <Text style={styles.modalTitle}>💖 เลือกความพึงพอใจในการจ่าย</Text>
            <Text style={styles.modalSubtitle}>สนับสนุนครีเอเตอร์และระบบตามคุณค่าที่คุณได้รับจริง</Text>

            <View style={styles.amountRow}>
              <TouchableOpacity style={styles.amountChip} onPress={() => handleValueContribution('50 บาท (ซาบซึ้ง)')}>
                <Text style={styles.amountChipText}>☕ 50 THB</Text>
              </TouchableOpacity>
              <TouchableOpacity style={styles.amountChip} onPress={() => handleValueContribution('150 บาท (ประทับใจ)')}>
                <Text style={styles.amountChipText}>🌟 150 THB</Text>
              </TouchableOpacity>
              <TouchableOpacity style={styles.amountChip} onPress={() => handleValueContribution('500 บาท (ทรงคุณค่า)')}>
                <Text style={styles.amountChipText}>💎 500 THB</Text>
              </TouchableOpacity>
            </View>

            <TextInput
              style={styles.input}
              placeholder="ระบุจำนวนเงินตามความพึงพอใจอื่นๆ..."
              placeholderTextColor="#9CA3AF"
              keyboardType="numeric"
              value={customAmount}
              onChangeText={setCustomAmount}
            />

            {customAmount ? (
              <TouchableOpacity style={styles.confirmButton} onPress={() => handleValueContribution(`${customAmount} บาท`)}>
                <Text style={styles.confirmButtonText}>ยืนยันการสนับสนุน ({customAmount} THB)</Text>
              </TouchableOpacity>
            ) : null}

            <TouchableOpacity style={styles.closeButton} onPress={() => setSupportModalVisible(false)}>
              <Text style={styles.closeButtonText}>ปิดหน้าต่าง</Text>
            </TouchableOpacity>
          </View>
        </View>
      </Modal>

    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 16 },
  header: { marginBottom: 10, alignItems: 'center' },
  motto: { fontSize: 10, color: '#9CA3AF', textAlign: 'center', marginBottom: 4, fontStyle: 'italic' },
  stationTitle: { fontSize: 18, fontWeight: 'bold' },
  stationDesc: { fontSize: 12, color: '#D1D5DB', textAlign: 'center', marginTop: 2 },
  viewport: { flex: 1, borderWidth: 2, borderRadius: 16, padding: 16, marginBottom: 10 },
  tabContent: { alignItems: 'center', paddingBottom: 20 },
  viewportTitle: { fontSize: 16, fontWeight: 'bold', marginBottom: 10, textAlign: 'center' },
  bodyText: { fontSize: 13, color: '#E5E7EB', textAlign: 'center', marginVertical: 4 },
  toolCard: { flexDirection: 'row', alignItems: 'center', backgroundColor: '#1F2937', borderWidth: 1, padding: 12, borderRadius: 12, marginVertical: 6, width: '100%' },
  toolEmoji: { fontSize: 24, marginRight: 12 },
  toolTextWrapper: { flex: 1 },
  toolTitle: { color: '#FFFFFF', fontSize: 13, fontWeight: 'bold' },
  toolSubtitle: { color: '#9CA3AF', fontSize: 11, marginTop: 2 },
  ambientToggleBtn: { marginTop: 12, paddingHorizontal: 16, paddingVertical: 10, borderRadius: 10, width: '100%', alignItems: 'center' },
  ambientToggleText: { fontWeight: 'bold', fontSize: 13 },
  supportButton: { marginTop: 16, paddingHorizontal: 20, paddingVertical: 12, borderRadius: 12 },
  supportButtonText: { fontWeight: 'bold', color: '#000', fontSize: 14 },
  stationSection: { marginBottom: 8 },
  menuLabel: { fontSize: 12, color: '#9CA3AF', marginBottom: 6 },
  stationList: { maxHeight: 45 },
  stationButton: { paddingHorizontal: 12, paddingVertical: 8, borderRadius: 8, marginRight: 6, justifyContent: 'center' },
  stationButtonText: { fontWeight: 'bold', fontSize: 12 },
  footer: { flexDirection: 'row', justifyContent: 'space-around', borderTopWidth: 1, borderTopColor: '#374151', paddingTop: 8 },
  footerTab: { paddingVertical: 6, paddingHorizontal: 12, borderRadius: 8 },
  activeTab: { backgroundColor: 'rgba(255, 255, 255, 0.1)' },
  footerText: { color: '#E5E7EB', fontSize: 13, fontWeight: '600' },
  modalOverlay: { flex: 1, justifyContent: 'flex-end', backgroundColor: 'rgba(0, 0, 0, 0.7)' },
  modalContent: { backgroundColor: '#1F2937', padding: 24, borderTopLeftRadius: 24, borderTopRightRadius: 24, alignItems: 'center' },
  modalTitle: { fontSize: 20, fontWeight: 'bold', color: '#FFFFFF', marginBottom: 6 },
  modalSubtitle: { fontSize: 13, color: '#9CA3AF', textAlign: 'center', marginBottom: 20 },
  amountRow: { flexDirection: 'row', justifyContent: 'space-between', width: '100%', marginBottom: 16 },
  amountChip: { backgroundColor: '#374151', paddingVertical: 10, paddingHorizontal: 12, borderRadius: 10, flex: 1, marginHorizontal: 4, alignItems: 'center' },
  amountChipText: { color: '#FFF', fontSize: 12, fontWeight: 'bold' },
  input: { width: '100%', backgroundColor: '#111827', color: '#FFF', padding: 12, borderRadius: 10, fontSize: 14, marginBottom: 16, borderWidth: 1, borderColor: '#4B5563' },
  confirmButton: { backgroundColor: '#10B981', width: '100%', padding: 14, borderRadius: 10, alignItems: 'center', marginBottom: 10 },
  confirmButtonText: { color: '#FFF', fontWeight: 'bold', fontSize: 14 },
  closeButton: { padding: 10 },
  closeButtonText: { color: '#9CA3AF', fontSize: 14 },
});
# Dexosapp
New app social media
