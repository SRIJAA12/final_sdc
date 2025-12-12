# 🗄️ DATABASE SETUP GUIDE - For Team Members

## ⚠️ IMPORTANT: Database is Shared!

Your MongoDB Atlas database is **already configured** and **cloud-based**. When your friend clones the project, they will connect to the **SAME database** you're using.

---

## 📊 Current Database Configuration

**Database Type:** MongoDB Atlas (Cloud)  
**Connection String:** Already in `app.js` (line 73)  
**Database Name:** `college-lab-registration`  
**Status:** ✅ Shared across all team members

---

## 🔧 Setup Instructions for Your Friend

### Step 1: Clone the Repository
```bash
git clone https://github.com/SRIJAA12/final_sdc.git
cd final_sdc
```

### Step 2: Install Dependencies

**For Server:**
```bash
cd central-admin/server
npm install
```

**For Kiosk:**
```bash
cd student-kiosk/desktop-app
npm install
```

### Step 3: Start the Server
```bash
cd central-admin/server
node app.js
```

**That's it!** Your friend will automatically connect to the same MongoDB Atlas database.

---

## ✅ What Your Friend Will See

When they start the server, they will see:
```
✅ MongoDB Connected: cluster0.2kzkkpe.mongodb.net
✅ Database: college-lab-registration
```

### They Will Have Access To:
- ✅ All students you added
- ✅ All timetables you uploaded
- ✅ All session history
- ✅ All system configurations
- ✅ Everything in the shared database!

---

## 🔒 Security Note

**⚠️ WARNING:** Your MongoDB credentials are currently **hardcoded** in `app.js` line 73. This means:
- Anyone who clones your repo can see the database password
- This is a **security risk** if the repo is public

### Recommended Fix: Use Environment Variables

1. Create a `.env` file in `central-admin/server/`:
```env
MONGODB_URI=mongodb+srv://srijaaanandhan12_db_user:122007@cluster0.2kzkkpe.mongodb.net/college-lab-registration?retryWrites=true&w=majority
PORT=7401
```

2. Add `.env` to `.gitignore` (already done ✅)

3. Update `app.js` to use only environment variable:
```javascript
const MONGODB_URI = process.env.MONGODB_URI || 'mongodb://localhost:27017/college-lab';
```

4. Share the `.env` file with your friend **privately** (via WhatsApp, email, NOT GitHub)

---

## 🆘 Troubleshooting

### Issue 1: "MongoServerError: Authentication failed"
**Cause:** MongoDB Atlas password incorrect  
**Solution:** Check the password in the connection string

### Issue 2: "MongooseServerSelectionError: Could not connect"
**Cause:** Network issue or IP not whitelisted  
**Solution:** 
1. Go to MongoDB Atlas → Network Access
2. Add IP address: `0.0.0.0/0` (allow all - for testing)
3. Or add your friend's specific IP address

### Issue 3: "Connection timed out"
**Cause:** Firewall blocking MongoDB port  
**Solution:**
1. Check firewall settings
2. Try different network (mobile hotspot)
3. Verify MongoDB Atlas cluster is running

### Issue 4: Friend sees empty database
**Cause:** Connected to different database or local MongoDB  
**Solution:**
1. Verify connection string matches exactly
2. Check server startup logs for database name
3. Run: `console.log(mongoose.connection.name)` to see connected DB

---

## 🌐 MongoDB Atlas IP Whitelist

Your friend needs to whitelist their IP in MongoDB Atlas:

1. Go to: https://cloud.mongodb.com/
2. Login with your MongoDB account
3. Click on your cluster (Cluster0)
4. Go to **Network Access** tab
5. Click **Add IP Address**
6. Either:
   - Add their specific IP address
   - Or add `0.0.0.0/0` to allow from anywhere (less secure but easier)

---

## 📝 Current Database Collections

Your shared database contains:

1. **students** - All registered students
2. **sessions** - Login/logout history
3. **systemregistry** - System status tracking
4. **guestsessions** - Guest access records
5. **timetables** - Lab schedules
6. **otps** - Password reset OTPs

**All of these are shared!** Changes made by you or your friend will be visible to both.

---

## 🔄 Data Synchronization

**How it works:**
- ✅ Real-time: Changes are instant (thanks to MongoDB Atlas)
- ✅ Automatic: No manual sync needed
- ✅ Persistent: Data survives server restarts
- ✅ Shared: Both users see the same data

**Example:**
1. You add a student → Database updated immediately
2. Your friend refreshes admin dashboard → Sees the new student
3. Your friend deletes the student → You see it's gone
4. Student changes password on kiosk → Updated for everyone

---

## 💡 Important Notes

### ✅ What IS Shared (Database Data):
- Student accounts
- Session history
- Timetables
- Guest sessions
- System configurations

### ❌ What is NOT Shared (Local Files):
- Server logs (`server-log.txt`)
- Uploaded CSV files (`reports/`, `session-csvs/`)
- Screenshots/screen recordings
- Local configuration files

### 🔄 To Share Local Files:
If you need to share reports or logs, use:
- Google Drive
- OneDrive
- Direct file sharing
- Or commit them to Git (not recommended for large files)

---

## 🚀 Quick Test for Your Friend

After setup, run this test to verify database connection:

```javascript
// In central-admin/server/, create test-db.js:
const mongoose = require('mongoose');

const MONGODB_URI = 'mongodb+srv://srijaaanandhan12_db_user:122007@cluster0.2kzkkpe.mongodb.net/college-lab-registration?retryWrites=true&w=majority';

mongoose.connect(MONGODB_URI)
  .then(() => {
    console.log('✅ Connected to MongoDB Atlas!');
    console.log('Database:', mongoose.connection.name);
    
    // Count students
    const Student = mongoose.model('Student', new mongoose.Schema({}), 'students');
    return Student.countDocuments();
  })
  .then(count => {
    console.log(`✅ Found ${count} students in database`);
    console.log('Database is working correctly!');
    process.exit(0);
  })
  .catch(err => {
    console.error('❌ Connection failed:', err.message);
    process.exit(1);
  });
```

Run test:
```bash
node test-db.js
```

Expected output:
```
✅ Connected to MongoDB Atlas!
Database: college-lab-registration
✅ Found X students in database
Database is working correctly!
```

---

## 🎯 Summary

### For You (Original Developer):
- ✅ Your database is already cloud-based (MongoDB Atlas)
- ✅ No changes needed on your side
- ✅ Continue working as usual

### For Your Friend:
1. Clone the repo
2. Install dependencies (`npm install`)
3. Start server (`node app.js`)
4. **Done!** - Automatically connects to your shared database

### For Both:
- ✅ Changes are visible to everyone
- ✅ Real-time synchronization
- ✅ No manual sync required
- ⚠️ Be careful - deleting data affects everyone!

---

## 🔐 Security Recommendations

### Before Public Deployment:

1. **Create separate .env file** (don't commit credentials)
2. **Restrict IP access** in MongoDB Atlas (not 0.0.0.0/0)
3. **Use separate databases** for development and production
4. **Rotate MongoDB password** periodically
5. **Enable MongoDB Atlas audit logs**

---

## 📞 Need Help?

If your friend faces issues:

1. Check MongoDB Atlas is running
2. Verify IP is whitelisted
3. Check connection string is correct
4. Test with the test script above
5. Check firewall/antivirus settings

---

**✅ Your database is already set up for sharing!**  
**✅ Your friend just needs to clone and run!**  
**✅ No additional configuration needed!**

---

**Last Updated:** December 12, 2025  
**MongoDB Atlas Cluster:** cluster0.2kzkkpe.mongodb.net  
**Database:** college-lab-registration  
**Status:** Active and Shared ✅
