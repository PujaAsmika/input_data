import { useState } from 'react';
import { 
  Search, 
  Menu, 
  RefreshCw, 
  ChevronRight, 
  LayoutDashboard, 
  PlusCircle, 
  History,
  Snowflake,
  Building2,
  Store,
  Home,
  Sparkles,
  Copy,
  ClipboardList,
  CheckCircle2,
  XCircle,
  Clock,
  Plus
} from 'lucide-react';
import { motion, AnimatePresence } from 'motion/react';

type ServiceStatus = 'Selesai' | 'Pending' | 'Gagal';
type ActiveView = 'Dashboard' | 'Lapor' | 'Riwayat';

interface ServiceReport {
  id: string;
  date: string;
  room: string;
  technician: {
    name: string;
    image: string;
  };
  status: ServiceStatus;
  type?: string;
  timeAgo?: string;
}

const MOCK_REPORTS: ServiceReport[] = [
  {
    id: '1',
    date: '24 Okt 2023',
    room: 'Gedung Graha Mandiri',
    technician: { name: 'Ahmad Subardjo', image: 'https://images.unsplash.com/photo-1560250097-0b93528c311a?w=100&h=100&fit=crop' },
    status: 'Selesai',
    type: 'Split Type',
    timeAgo: '20 Menit yang lalu'
  },
  {
    id: '2',
    date: '23 Okt 2023',
    room: 'Apartemen Sudirman',
    technician: { name: 'Budi Cahyono', image: 'https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=100&h=100&fit=crop' },
    status: 'Pending',
    type: 'Central AC',
    timeAgo: '1 Jam yang lalu'
  },
  {
    id: '3',
    date: '22 Okt 2023',
    room: 'IndoMarket Cab. 04',
    technician: { name: 'Siti Aminah', image: 'https://images.unsplash.com/photo-1573496359142-b8d87734a5a2?w=100&h=100&fit=crop' },
    status: 'Gagal',
    type: 'Floor Standing',
    timeAgo: '3 Jam yang lalu'
  },
  {
    id: '4',
    date: '21 Okt 2023',
    room: 'Residensi Permata',
    technician: { name: 'Ahmad Subardjo', image: 'https://images.unsplash.com/photo-1560250097-0b93528c311a?w=100&h=100&fit=crop' },
    status: 'Selesai',
    type: 'Multi Split',
    timeAgo: '5 Jam yang lalu'
  }
];

export default function App() {
  const [activeView, setActiveView] = useState<ActiveView>('Dashboard');
  const [activeFilter, setActiveFilter] = useState<'Semua' | ServiceStatus>('Semua');
  const [searchQuery, setSearchQuery] = useState('');

  return (
    <div className="min-h-screen bg-surface flex flex-col pb-safe">
      {/* Top Bar */}
      <header className="sticky top-0 z-50 bg-white/80 backdrop-blur-md border-b border-surface-container-highest px-4 h-14 flex items-center justify-between">
        <div className="flex items-center gap-3">
          <Menu className="w-6 h-6 text-primary cursor-pointer" />
          <h1 className="text-lg font-bold text-primary tracking-tight">Laporan Servis AC</h1>
        </div>
        <div className="w-8 h-8 rounded-full border border-surface-container-highest overflow-hidden shadow-sm">
          <img src="https://images.unsplash.com/photo-1560250097-0b93528c311a?w=100&h=100&fit=crop" alt="User" className="w-full h-full object-cover" />
        </div>
      </header>

      <main className="flex-1 overflow-y-auto px-4 py-6 max-w-lg mx-auto w-full relative">
        <AnimatePresence mode="wait">
          {activeView === 'Dashboard' && (
            <DashboardView key="dashboard" onLaporClick={() => setActiveView('Lapor')} />
          )}
          {activeView === 'Lapor' && (
            <LaporView key="lapor" />
          )}
          {activeView === 'Riwayat' && (
            <RiwayatView 
              key="riwayat"
              searchQuery={searchQuery}
              setSearchQuery={setSearchQuery}
              activeFilter={activeFilter}
              setActiveFilter={setActiveFilter}
            />
          )}
        </AnimatePresence>
      </main>

      {/* Bottom Nav */}
      <nav className="fixed bottom-0 left-0 right-0 z-50 bg-white border-t border-surface-container-highest px-8 py-2 pb-6 flex justify-around items-center">
        <NavItem 
          onClick={() => setActiveView('Dashboard')}
          icon={<LayoutDashboard className="w-6 h-6" />} 
          label="Dashboard" 
          active={activeView === 'Dashboard'} 
        />
        <NavItem 
          onClick={() => setActiveView('Lapor')}
          icon={<PlusCircle className="w-6 h-6" />} 
          label="Lapor" 
          active={activeView === 'Lapor'} 
        />
        <NavItem 
          onClick={() => setActiveView('Riwayat')}
          icon={<History className="w-6 h-6" />} 
          label="Riwayat" 
          active={activeView === 'Riwayat'} 
        />
      </nav>
    </div>
  );
}

function DashboardView({ onLaporClick }: { onLaporClick: () => void }) {
  return (
    <motion.div 
      initial={{ opacity: 0, x: 20 }}
      animate={{ opacity: 1, x: 0 }}
      exit={{ opacity: 0, x: -20 }}
      className="space-y-6"
    >
      {/* Stat Cards */}
      <div className="space-y-4">
        <div className="bg-white border border-surface-container-highest rounded-2xl p-5 shadow-sm">
          <p className="text-sm font-semibold text-on-surface-variant mb-1">Total Unit Servis</p>
          <div className="flex items-end justify-between">
            <h2 className="text-4xl font-bold text-primary">128</h2>
            <span className="px-3 py-1 bg-primary/10 text-primary rounded-full text-xs font-bold">Bulan Ini</span>
          </div>
        </div>

        <div className="grid grid-cols-2 gap-4">
          <div className="bg-white border border-surface-container-highest rounded-2xl p-5 shadow-sm">
            <div className="flex items-center gap-2 text-status-success mb-2">
              <CheckCircle2 className="w-5 h-5" />
              <span className="text-sm font-semibold">Selesai</span>
            </div>
            <h2 className="text-3xl font-bold text-on-surface">114</h2>
          </div>
          <div className="bg-white border border-surface-container-highest rounded-2xl p-5 shadow-sm">
            <div className="flex items-center gap-2 text-status-error mb-2">
              <XCircle className="w-5 h-5" />
              <span className="text-sm font-semibold">Gagal</span>
            </div>
            <h2 className="text-3xl font-bold text-on-surface">14</h2>
          </div>
        </div>
      </div>

      {/* Recent Reports */}
      <div className="space-y-4 pt-4">
        <div className="flex items-center justify-between">
          <h3 className="text-lg font-bold text-primary">Laporan Terbaru</h3>
          <button className="text-sm font-bold text-primary hover:underline">Lihat Semua</button>
        </div>

        <div className="space-y-3">
          {MOCK_REPORTS.map((report) => (
            <div key={report.id} className="bg-white border border-surface-container-highest rounded-2xl p-4 flex items-center gap-4 relative group hover:border-primary/30 transition-all">
              <div className={`p-3 rounded-xl ${
                report.room.includes('Gedung') ? 'bg-blue-50 text-blue-600' :
                report.room.includes('Apartemen') ? 'bg-indigo-50 text-indigo-600' :
                report.room.includes('IndoMarket') ? 'bg-violet-50 text-violet-600' : 'bg-slate-50 text-slate-600'
              }`}>
                {report.room.includes('Gedung') ? <Snowflake className="w-6 h-6" /> :
                 report.room.includes('Apartemen') ? <Building2 className="w-6 h-6" /> :
                 report.room.includes('IndoMarket') ? <Store className="w-6 h-6" /> : <Home className="w-6 h-6" />}
              </div>
              <div className="flex-1 min-w-0">
                <div className="flex items-start justify-between mb-0.5">
                  <h4 className="font-bold text-on-surface truncate">{report.room}</h4>
                  <span className={`px-2 py-0.5 rounded-lg text-[10px] font-bold uppercase ${
                    report.status === 'Selesai' ? 'bg-green-100 text-green-700' :
                    report.status === 'Pending' ? 'bg-orange-100 text-orange-700' : 'bg-red-100 text-red-700'
                  }`}>
                    {report.status}
                  </span>
                </div>
                <p className="text-xs text-on-surface-variant font-medium">
                  {report.timeAgo} • {report.type}
                </p>
              </div>
              <ChevronRight className="w-4 h-4 text-on-surface-variant/40" />
            </div>
          ))}
        </div>
      </div>

      {/* FAB */}
      <motion.button
        whileHover={{ scale: 1.05 }}
        whileTap={{ scale: 0.95 }}
        onClick={onLaporClick}
        className="fixed bottom-24 right-4 w-14 h-14 bg-primary text-white rounded-2xl shadow-xl flex items-center justify-center z-40"
      >
        <Plus className="w-8 h-8" />
      </motion.button>
    </motion.div>
  );
}

function LaporView() {
  const [chatLog, setChatLog] = useState('');
  const [isProcessing, setIsProcessing] = useState(false);
  const [preview, setPreview] = useState<any>(null);

  const handleProcess = async () => {
    if (!chatLog) return;
    setIsProcessing(true);
    try {
      const res = await fetch('/api/process-chat', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ chatLog })
      });
      const data = await res.json();
      setPreview(data);
    } catch (e) {
      console.error(e);
    } finally {
      setIsProcessing(false);
    }
  };

  return (
    <motion.div 
      initial={{ opacity: 0, scale: 0.95 }}
      animate={{ opacity: 1, scale: 1 }}
      exit={{ opacity: 0, scale: 0.95 }}
      className="space-y-6 pb-12"
    >
      <div className="space-y-4">
        <label className="flex items-center justify-between text-[10px] font-bold text-on-surface-variant uppercase tracking-widest">
          <span>INPUT CHAT LOG</span>
          <span className="text-primary/60">AUTO-DETECT ACTIVE</span>
        </label>
        <textarea 
          value={chatLog}
          onChange={(e) => setChatLog(e.target.value)}
          placeholder="Paste chat history from WhatsApp or Telegram here..."
          className="w-full h-48 bg-white border border-surface-container-highest rounded-2xl p-4 text-sm focus:ring-2 focus:ring-primary/20 focus:border-primary outline-none transition-all shadow-sm"
        />
        <motion.button 
          whileTap={{ scale: 0.98 }}
          onClick={handleProcess}
          disabled={isProcessing || !chatLog}
          className="w-full bg-primary text-white py-4 rounded-xl font-bold flex items-center justify-center gap-3 shadow-lg hover:bg-primary-container transition-colors disabled:opacity-50"
        >
          {isProcessing ? (
            <RefreshCw className="w-5 h-5 animate-spin" />
          ) : (
            <>
              <Sparkles className="w-5 h-5" />
              Proses dengan AI
            </>
          )}
        </motion.button>
      </div>

      {preview && (
        <div className="space-y-4 animate-in fade-in slide-in-from-bottom-4">
          <div className="flex items-center gap-2 text-[10px] font-bold text-on-surface-variant uppercase tracking-widest">
            <Clock className="w-4 h-4" />
            LIVE PREVIEW
          </div>
          <div className="bg-white border border-surface-container-highest rounded-2xl p-6 shadow-sm space-y-6">
            <div className="flex justify-between items-start border-b border-surface-container-highest pb-4">
              <div>
                <p className="text-[10px] font-bold text-on-surface-variant uppercase mb-1">DATE</p>
                <p className="text-sm font-bold text-on-surface">{preview.date || '24 Oct 2023'}</p>
              </div>
              <div className="text-right">
                <span className="bg-secondary-container text-primary text-[10px] font-bold px-2 py-1 rounded">DRAFT</span>
                <p className="text-sm font-bold text-on-surface mt-2">{preview.room || 'Server RM 202'}</p>
              </div>
            </div>

            <div className="grid grid-cols-2 gap-4">
              <div>
                <p className="text-[10px] font-bold text-on-surface-variant uppercase mb-1">CATEGORY</p>
                <p className="text-xs font-bold text-on-surface">{preview.category || 'General Wash'}</p>
              </div>
              <div className="text-right">
                <p className="text-[10px] font-bold text-on-surface-variant uppercase mb-1">TECHNICIAN</p>
                <p className="text-xs font-bold text-on-surface">{preview.technician || 'Andi Wijaya'}</p>
              </div>
            </div>

            <div>
              <p className="text-[10px] font-bold text-on-surface-variant uppercase mb-1">STATUS</p>
              <span className={`px-3 py-1 rounded bg-green-100 text-green-700 text-[10px] font-bold border border-green-200`}>
                {preview.status?.toUpperCase() || 'SELESAI'}
              </span>
            </div>

            <div>
              <p className="text-[10px] font-bold text-on-surface-variant uppercase mb-1">NOTES</p>
              <p className="text-xs text-on-surface-variant italic leading-relaxed">
                "{preview.notes || 'Unit AC merk Daikin 2PK, tekanan freon normal (140psi), cuci filter dan evaporator, drainase lancar kembali.'}"
              </p>
            </div>
          </div>

          <div className="grid grid-cols-2 gap-3">
            <button className="flex items-center justify-center gap-2 bg-surface-container-high border border-surface-container-highest py-3 rounded-xl text-xs font-bold text-primary">
              <ClipboardList className="w-4 h-4" />
              Copy Markdown Table
            </button>
            <button className="flex items-center justify-center gap-2 bg-surface-container-high border border-surface-container-highest py-3 rounded-xl text-xs font-bold text-primary">
              <Copy className="w-4 h-4" />
              Copy CSV Format
            </button>
          </div>
        </div>
      )}
    </motion.div>
  );
}

function RiwayatView({ searchQuery, setSearchQuery, activeFilter, setActiveFilter }: any) {
  const filteredReports = MOCK_REPORTS.filter(report => {
    const matchesFilter = activeFilter === 'Semua' || report.status === activeFilter;
    const matchesSearch = report.room.toLowerCase().includes(searchQuery.toLowerCase()) || 
                         report.technician.name.toLowerCase().includes(searchQuery.toLowerCase());
    return matchesFilter && matchesSearch;
  });

  return (
    <motion.div 
      initial={{ opacity: 0, x: -20 }}
      animate={{ opacity: 1, x: 0 }}
      exit={{ opacity: 0, x: 20 }}
      className="space-y-6"
    >
      <div className="relative">
        <Search className="absolute left-4 top-1/2 -translate-y-1/2 w-5 h-5 text-on-surface-variant" />
        <input 
          type="text"
          placeholder="Cari laporan atau teknisi..."
          className="w-full h-12 bg-white border border-surface-container-highest rounded-xl pl-12 pr-4 focus:border-primary outline-none transition-all shadow-sm text-sm"
          value={searchQuery}
          onChange={(e) => setSearchQuery(e.target.value)}
        />
      </div>

      <div className="flex gap-2 overflow-x-auto hide-scrollbar">
        {(['Semua', 'Selesai', 'Pending', 'Gagal'] as const).map((filter) => (
          <button
            key={filter}
            onClick={() => setActiveFilter(filter)}
            className={`px-4 py-2 rounded-full text-xs font-bold whitespace-nowrap border transition-all ${
              activeFilter === filter 
                ? 'bg-primary text-white border-primary shadow-md' 
                : 'bg-white text-on-surface-variant border-surface-container-highest hover:bg-surface-container-low'
            }`}
          >
            {filter}
          </button>
        ))}
      </div>

      <div className="space-y-4">
        {filteredReports.map((report) => (
          <div key={report.id} className="bg-white border border-surface-container-highest rounded-2xl p-4 space-y-3 shadow-sm">
            <div className="flex justify-between items-start">
              <div className="space-y-1">
                <p className="text-[10px] font-bold text-on-surface-variant uppercase tracking-widest">TANGGAL: {report.date}</p>
                <h4 className="font-bold text-on-surface">{report.room}</h4>
              </div>
              <span className={`px-2 py-0.5 rounded text-[10px] font-bold ${
                report.status === 'Selesai' ? 'bg-green-100 text-green-700' :
                report.status === 'Pending' ? 'bg-orange-100 text-orange-700' : 'bg-red-100 text-red-700'
              }`}>
                {report.status}
              </span>
            </div>
            <div className="flex items-center gap-2">
              <img src={report.technician.image} className="w-5 h-5 rounded-full object-cover" />
              <span className="text-xs text-on-surface-variant font-medium">{report.technician.name}</span>
            </div>
            <div className="pt-2 flex justify-end">
              <button className="text-xs font-bold text-primary flex items-center gap-1">
                Detail <ChevronRight className="w-4 h-4" />
              </button>
            </div>
          </div>
        ))}
      </div>
    </motion.div>
  );
}

function NavItem({ icon, label, active, onClick }: { icon: React.ReactNode, label: string, active: boolean, onClick: () => void }) {
  return (
    <button
      onClick={onClick}
      className={`flex flex-col items-center gap-1 transition-all ${active ? 'text-primary' : 'text-on-surface-variant hover:text-primary'}`}
    >
      <div className={`p-2 rounded-xl transition-all ${active ? 'bg-secondary-container shadow-sm' : ''}`}>
        {icon}
      </div>
      <span className="text-[10px] font-bold uppercase tracking-widest">{label}</span>
    </button>
  );
}
