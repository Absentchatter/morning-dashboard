import React, { useState, useEffect } from 'react';

export default function MorningDashboard() {
  const [refreshing, setRefreshing] = useState(false);
  const [refreshStatus, setRefreshStatus] = useState('');
  const [lastRefresh, setLastRefresh] = useState(new Date());
  const [notionSyncStatus, setNotionSyncStatus] = useState('');
  const [emailTasks, setEmailTasks] = useState([
    {
      id: 'e1',
      from: 'ADPClientServices@adp.com',
      subject: 'Employee License Expiration',
      why: 'River Murphy\'s SLP-MI state licensure expires Aug 19, 2026. Reach out to affected employees.',
      actionText: 'Review & Notify'
    },
    {
      id: 'e2',
      from: 'c.halbower@globalpsychological.com',
      subject: 'Re: RFP Summaries — North Carolina pay rates',
      why: 'Courtney provided NC therapist pay data (PT $40/hr, OT $41/hr). Compile WV + NC and respond.',
      actionText: 'Compile & Reply'
    },
    {
      id: 'e3',
      from: 'notification@monday-mail.com',
      subject: 'New Protocol Request — By May 1',
      why: 'Claire Olech submitted a protocol request. Items needed by May 1, 2026.',
      actionText: 'Review'
    },
    {
      id: 'e4',
      from: 'V.Exner@globalpsychological.com',
      subject: 'Re: IEP Goal Bank',
      why: 'Vance flagged the IEP Goal Bank needs summer expansion before August school year.',
      actionText: 'Plan & Schedule'
    }
  ]);

  const [dismissedItems, setDismissedItems] = useState(() => {
    const stored = localStorage.getItem('dismissedEmailTasks');
    return stored ? JSON.parse(stored) : {};
  });

  const [addedToNotionIds, setAddedToNotionIds] = useState(() => {
    const stored = localStorage.getItem('addedToNotionTasks');
    return stored ? JSON.parse(stored) : new Set();
  });

  const [newsLinks, setNewsLinks] = useState([
    { source: 'BH Business', title: 'BH sector shifting from growth to proof — measurement-based care, AI tools, provider value core priorities', link: 'https://bhbusiness.com/2025/12/31/' },
    { source: 'AAPC Healthcare', title: '2026 BH outlook: AI-based claim denials rising, mental health parity gaining traction', link: 'https://www.aapc.com/blog/' },
    { source: 'K-12 Dive', title: 'Court orders school mental health grants reinstatement in 16 states; project-by-project review underway', link: 'https://www.k12dive.com/' }
  ]);

  // Check if item should be hidden (dismissed < 72 hours ago)
  const isItemHidden = (itemId) => {
    const dismissedTime = dismissedItems[itemId];
    if (!dismissedTime) return false;
    const now = Date.now();
    const seventyTwoHours = 72 * 60 * 60 * 1000;
    return (now - dismissedTime) < seventyTwoHours;
  };

  // Filter visible tasks
  const visibleTasks = emailTasks.filter(task => {
    const isAdded = addedToNotionIds.has(task.id);
    const isDismissed = isItemHidden(task.id);
    return !isAdded && !isDismissed;
  });

  // Handle refresh
  const handleRefresh = async () => {
    setRefreshing(true);
    setRefreshStatus('🔄 Syncing Notion...');
    
    try {
      await new Promise(r => setTimeout(r, 800));
      setRefreshStatus('📧 Scanning emails...');
      
      await new Promise(r => setTimeout(r, 600));
      setRefreshStatus('📰 Loading headlines...');
      
      await new Promise(r => setTimeout(r, 400));
      setRefreshStatus('✨ Ready!');
      setLastRefresh(new Date());
      
      await new Promise(r => setTimeout(r, 1500));
    } finally {
      setRefreshing(false);
      setRefreshStatus('');
    }
  };

  // Add task to Notion
  const handleAddToNotion = async (taskId, taskTitle) => {
    setNotionSyncStatus(`Adding "${taskTitle}" to Notion...`);
    
    try {
      // Simulate API call to add to Notion
      await new Promise(r => setTimeout(r, 1200));
      
      const newAdded = new Set(addedToNotionIds);
      newAdded.add(taskId);
      setAddedToNotionIds(newAdded);
      localStorage.setItem('addedToNotionTasks', JSON.stringify(Array.from(newAdded)));
      
      setNotionSyncStatus(`✅ Added to Notion!`);
      setTimeout(() => setNotionSyncStatus(''), 2000);
    } catch (error) {
      setNotionSyncStatus(`❌ Error: ${error.message}`);
      setTimeout(() => setNotionSyncStatus(''), 3000);
    }
  };

  // Dismiss item (72-hour hide)
  const handleDismiss = (taskId) => {
    const newDismissed = { ...dismissedItems, [taskId]: Date.now() };
    setDismissedItems(newDismissed);
    localStorage.setItem('dismissedEmailTasks', JSON.stringify(newDismissed));
  };

  const formatTime = (date) => {
    const now = new Date();
    const diff = now - date;
    const minutes = Math.floor(diff / 60000);
    if (minutes < 1) return 'just now';
    if (minutes < 60) return `${minutes}m ago`;
    const hours = Math.floor(diff / 3600000);
    if (hours < 24) return `${hours}h ago`;
    return date.toLocaleDateString();
  };

  return (
    <div style={styles.container}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        * { font-family: 'Inter', system-ui, sans-serif; }
        @keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: 0.5; } }
        @keyframes slideIn { from { opacity: 0; transform: translateY(-8px); } to { opacity: 1; transform: translateY(0); } }
        .pulse { animation: pulse 1.5s ease-in-out infinite; }
        .slideIn { animation: slideIn 0.3s ease-out; }
      `}</style>

      {/* Header Bar */}
      <div style={styles.header}>
        <div style={styles.headerLeft}>
          <div style={styles.greetingText}>
            Good morning, Zach! <span style={styles.timestamp}>{formatTime(lastRefresh)}</span>
          </div>
        </div>
        <button 
          style={{ ...styles.refreshBtn, ...(refreshing ? styles.refreshBtnActive : {}) }}
          onClick={handleRefresh}
          disabled={refreshing}
        >
          {refreshing ? '⏳ Refreshing...' : '🔄 Refresh All'}
        </button>
      </div>

      {/* Refresh Status */}
      {refreshStatus && (
        <div style={{ ...styles.statusBanner, animation: 'slideIn 0.3s ease-out' }}>
          <span className="pulse">●</span> {refreshStatus}
        </div>
      )}

      {/* Notion Sync Status */}
      {notionSyncStatus && (
        <div style={{ ...styles.statusBanner, animation: 'slideIn 0.3s ease-out' }}>
          {notionSyncStatus}
        </div>
      )}

      {/* Two-Column Grid */}
      <div style={styles.grid}>
        {/* Left: Tasks */}
        <div style={styles.card}>
          <div style={styles.cardHeader}>
            <h3 style={styles.cardTitle}>📋 Notion Tasks</h3>
            <span style={styles.badge}>4 tasks</span>
          </div>
          <div style={styles.taskList}>
            <div style={styles.taskItem}>
              <div><strong>Questions for Social Work Interview</strong></div>
              <div style={styles.taskMeta}>Due Today (Apr 21) — ⚠️ Still "To Do"</div>
            </div>
            <div style={styles.taskItem}>
              <div><strong>Ottawa Billing 3.16–4.17</strong></div>
              <div style={styles.taskMeta}>Due Apr 22 — In Progress</div>
            </div>
            <div style={styles.taskItem}>
              <div><strong>Paint Glottkin</strong></div>
              <div style={styles.taskMeta}>Added Apr 20 — To Do</div>
            </div>
          </div>
          <div style={styles.syncInfo}>Last synced: {formatTime(lastRefresh)}</div>
        </div>

        {/* Right: News */}
        <div style={styles.card}>
          <div style={styles.cardHeader}>
            <h3 style={styles.cardTitle}>📰 Industry News</h3>
          </div>
          <div style={styles.newsList}>
            {newsLinks.map((item, i) => (
              <div key={i} style={styles.newsItem}>
                <div style={styles.newsSource}>{item.source}</div>
                <a href={item.link} target="_blank" rel="noopener noreferrer" style={styles.newsLink}>
                  {item.title}
                </a>
              </div>
            ))}
          </div>
        </div>
      </div>

      {/* Email Action Items */}
      <div style={{ ...styles.card, gridColumn: '1 / -1' }}>
        <div style={styles.cardHeader}>
          <h3 style={styles.cardTitle}>
            ✉️ Email Action Items
            {visibleTasks.length > 0 && <span style={styles.actionCount}>{visibleTasks.length} pending</span>}
          </h3>
        </div>
        
        {visibleTasks.length === 0 ? (
          <div style={styles.emptyState}>
            ✅ All caught up! No pending email tasks. 
            {Object.keys(dismissedItems).length > 0 && (
              <div style={{ fontSize: '12px', marginTop: '8px', color: '#9a9990' }}>
                {Object.values(dismissedItems).filter(t => isItemHidden(Object.keys(dismissedItems)[Object.values(dismissedItems).indexOf(t)])).length} items hidden for 72 hours
              </div>
            )}
          </div>
        ) : (
          <div style={styles.emailList}>
            {visibleTasks.map(task => (
              <div key={task.id} style={{ ...styles.emailItem, animation: 'slideIn 0.3s ease-out' }}>
                <div style={styles.emailFrom}>{task.from}</div>
                <div style={styles.emailSubject}>{task.subject}</div>
                <div style={styles.emailWhy}>{task.why}</div>
                <div style={styles.emailActions}>
                  <button 
                    style={styles.addBtn}
                    onClick={() => handleAddToNotion(task.id, task.subject)}
                  >
                    ➕ {task.actionText} → Notion
                  </button>
                  <button 
                    style={styles.dismissBtn}
                    onClick={() => handleDismiss(task.id)}
                  >
                    🙈 Hide 72h
                  </button>
                </div>
              </div>
            ))}
          </div>
        )}
      </div>

      {/* Stats Footer */}
      <div style={styles.statsRow}>
        <div style={styles.stat}>
          <div style={styles.statLabel}>Notion Tasks</div>
          <div style={styles.statValue}>4</div>
        </div>
        <div style={styles.stat}>
          <div style={styles.statLabel}>Pending Email Actions</div>
          <div style={styles.statValue}>{visibleTasks.length}</div>
        </div>
        <div style={styles.stat}>
          <div style={styles.statLabel}>Added to Notion</div>
          <div style={styles.statValue}>{addedToNotionIds.size}</div>
        </div>
        <div style={styles.stat}>
          <div style={styles.statLabel}>Hidden (72h)</div>
          <div style={styles.statValue}>{Object.keys(dismissedItems).length}</div>
        </div>
      </div>
    </div>
  );
}

const styles = {
  container: {
    padding: '24px',
    background: '#f5f4f0',
    minHeight: '100vh',
    fontFamily: "'Inter', system-ui, sans-serif",
  },
  header: {
    display: 'flex',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: '16px',
    padding: '12px 16px',
    background: '#eaf5ee',
    border: '0.5px solid rgba(26,107,53,0.15)',
    borderRadius: '8px',
  },
  headerLeft: {
    flex: 1,
  },
  greetingText: {
    fontSize: '14px',
    fontWeight: '600',
    color: '#1a6b35',
  },
  timestamp: {
    fontSize: '12px',
    color: '#9a9990',
    fontWeight: '400',
    marginLeft: '8px',
  },
  refreshBtn: {
    padding: '6px 14px',
    fontSize: '12px',
    fontWeight: '500',
    border: '0.5px solid rgba(0,0,0,0.1)',
    borderRadius: '6px',
    background: 'transparent',
    cursor: 'pointer',
    transition: 'all 0.2s',
  },
  refreshBtnActive: {
    background: '#f0f0f0',
    color: '#5a5a55',
  },
  statusBanner: {
    padding: '10px 14px',
    background: '#eaf5ee',
    border: '0.5px solid rgba(26,107,53,0.2)',
    borderRadius: '6px',
    fontSize: '12px',
    color: '#1a6b35',
    marginBottom: '12px',
    fontWeight: '500',
  },
  grid: {
    display: 'grid',
    gridTemplateColumns: '1fr 1fr',
    gap: '14px',
    marginBottom: '14px',
  },
  card: {
    background: '#fff',
    border: '0.5px solid rgba(0,0,0,0.08)',
    borderRadius: '10px',
    padding: '16px',
  },
  cardHeader: {
    display: 'flex',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: '12px',
  },
  cardTitle: {
    fontSize: '12px',
    fontWeight: '600',
    color: '#5a5a55',
    textTransform: 'uppercase',
    margin: 0,
    letterSpacing: '0.06em',
  },
  badge: {
    fontSize: '10px',
    padding: '2px 8px',
    borderRadius: '6px',
    background: '#eaf5ee',
    color: '#1a6b35',
    border: '0.5px solid rgba(26,107,53,0.2)',
  },
  taskList: {
    display: 'flex',
    flexDirection: 'column',
    gap: '8px',
  },
  taskItem: {
    fontSize: '13px',
    color: '#1a1a18',
    lineHeight: '1.4',
    paddingBottom: '8px',
    borderBottom: '0.5px solid rgba(0,0,0,0.06)',
  },
  taskMeta: {
    fontSize: '11px',
    color: '#9a9990',
    marginTop: '2px',
  },
  syncInfo: {
    fontSize: '11px',
    color: '#9a9990',
    marginTop: '8px',
    fontStyle: 'italic',
  },
  newsList: {
    display: 'flex',
    flexDirection: 'column',
    gap: '8px',
  },
  newsItem: {
    paddingBottom: '8px',
    borderBottom: '0.5px solid rgba(0,0,0,0.06)',
  },
  newsSource: {
    fontSize: '10px',
    color: '#9a9990',
    fontWeight: '500',
    marginBottom: '2px',
  },
  newsLink: {
    fontSize: '13px',
    color: '#1a1a18',
    textDecoration: 'none',
    lineHeight: '1.45',
  },
  emailList: {
    display: 'flex',
    flexDirection: 'column',
    gap: '12px',
  },
  emailItem: {
    border: '0.5px solid rgba(0,0,0,0.06)',
    borderRadius: '8px',
    padding: '12px',
    background: '#fafaf8',
  },
  emailFrom: {
    fontSize: '10px',
    color: '#9a9990',
    fontWeight: '500',
    marginBottom: '4px',
  },
  emailSubject: {
    fontSize: '13px',
    fontWeight: '600',
    color: '#1a1a18',
    marginBottom: '4px',
  },
  emailWhy: {
    fontSize: '12px',
    color: '#5a5a55',
    lineHeight: '1.5',
    marginBottom: '10px',
  },
  emailActions: {
    display: 'flex',
    gap: '8px',
    flexWrap: 'wrap',
  },
  addBtn: {
    fontSize: '11px',
    padding: '6px 12px',
    border: '0.5px solid rgba(26,61,143,0.2)',
    borderRadius: '6px',
    background: '#f0f5ff',
    color: '#1a3d8f',
    cursor: 'pointer',
    fontWeight: '500',
    transition: 'all 0.2s',
  },
  dismissBtn: {
    fontSize: '11px',
    padding: '6px 10px',
    border: '0.5px solid rgba(0,0,0,0.1)',
    borderRadius: '6px',
    background: 'transparent',
    color: '#9a9990',
    cursor: 'pointer',
    transition: 'all 0.2s',
  },
  emptyState: {
    textAlign: 'center',
    padding: '24px 16px',
    color: '#5a5a55',
    fontSize: '13px',
  },
  actionCount: {
    fontSize: '11px',
    marginLeft: '8px',
    color: '#8f1a1a',
    background: '#fdeaea',
    padding: '2px 6px',
    borderRadius: '99px',
    fontWeight: '500',
  },
  statsRow: {
    display: 'grid',
    gridTemplateColumns: 'repeat(4, 1fr)',
    gap: '8px',
  },
  stat: {
    background: '#fafaf8',
    borderRadius: '8px',
    padding: '12px',
    border: '0.5px solid rgba(0,0,0,0.04)',
  },
  statLabel: {
    fontSize: '11px',
    color: '#9a9990',
    marginBottom: '4px',
  },
  statValue: {
    fontSize: '20px',
    fontWeight: '600',
    color: '#1a1a18',
  },
};
