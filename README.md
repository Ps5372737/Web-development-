# Web-development-
const withPWA = require('next-pwa')({
  dest: 'public',
});

module.exports = withPWA({
  // your next.js config
  reactStrictMode: true,
});
{
  "name": "Admin Dashboard",
  "short_name": "Dashboard",
  "description": "An admin dashboard built with Next.js",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "icons": [
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
self.addEventListener('install', event => {
  console.log('Service worker installed');
});

self.addEventListener('activate', event => {
  console.log('Service worker activated');
});

self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request).then(response => {
      return response || fetch(event.request);
    })
  );
});
import { Button } from '@nextui-org/react';

export default function Home() {
  return (
    <div>
      <h1>Admin Dashboard</h1>
      <Button>Send Notification</Button>
    </div>
  );
}
import { useState } from 'react';
import { Table, Button, Input, Modal } from '@nextui-org/react';

const UserManagement = () => {
  const [users, setUsers] = useState([]);
  const [modalOpen, setModalOpen] = useState(false);
  const [editUser, setEditUser] = useState(null);

  const addUser = (user) => setUsers([...users, user]);
  const deleteUser = (id) => setUsers(users.filter(user => user.id !== id));
  const editExistingUser = (user) => {
    setUsers(users.map(u => (u.id === user.id ? user : u)));
    setEditUser(null);
  };

  return (
    <div>
      <Button onClick={() => setModalOpen(true)}>Add User</Button>
      <Table>
        <Table.Header>
          <Table.Column>Name</Table.Column>
          <Table.Column>Email</Table.Column>
          <Table.Column>Actions</Table.Column>
        </Table.Header>
        <Table.Body>
          {users.map(user => (
            <Table.Row key={user.id}>
              <Table.Cell>{user.name}</Table.Cell>
              <Table.Cell>{user.email}</Table.Cell>
              <Table.Cell>
                <Button onClick={() => setEditUser(user)}>Edit</Button>
                <Button onClick={() => deleteUser(user.id)}>Delete</Button>
              </Table.Cell>
            </Table.Row>
          ))}
        </Table.Body>
      </Table>
      {modalOpen && (
        <Modal open={modalOpen} onClose={() => setModalOpen(false)}>
          <Modal.Header>Add User</Modal.Header>
          <Modal.Body>
            <Input placeholder="Name" onChange={e => setEditUser({ ...editUser, name: e.target.value })} />
            <Input placeholder="Email" onChange={e => setEditUser({ ...editUser, email: e.target.value })} />
          </Modal.Body>
          <Modal.Footer>
            <Button onClick={() => { addUser(editUser); setModalOpen(false); }}>Add</Button>
          </Modal.Footer>
        </Modal>
      )}
    </div>
  );
};

export default UserManagement;
import { Card, Text } from '@nextui-org/react';

const Analytics = ({ metrics }) => (
  <div>
    <Card>
      <Text>User Count: {metrics.userCount}</Text>
    </Card>
    <Card>
      <Text>Active Users: {metrics.activeUsers}</Text>
    </Card>
    <Card>
      <Text>Revenue: ${metrics.revenue}</Text>
    </Card>
  </div>
);

export default Analytics;
import { Button, Card, Text } from '@nextui-org/react';

const Notifications = () => {
  const sendNotification = () => {
    if (Notification.permission === 'granted') {
      new Notification('This is a test notification');
    }
  };

  return (
    <Card>
      <Text>Notifications</Text>
      <Button onClick={sendNotification}>Send Notification</Button>
    </Card>
  );
};

export default Notifications;
import { Input, Button } from '@nextui-org/react';

const Settings = () => (
  <div>
    <h2>Settings</h2>
    <Input placeholder="App Name" />
    <Button>Save</Button>
  </div>
);

export default Settings;
import { useState } from 'react';
import { Table, Button, Input, Modal } from '@nextui-org/react';

const TaskManagement = () => {
  const [tasks, setTasks] = useState([]);
  const [modalOpen, setModalOpen] = useState(false);
  const [editTask, setEditTask] = useState(null);

  const addTask = (task) => setTasks([...tasks, task]);
  const deleteTask = (id) => setTasks(tasks.filter(task => task.id !== id));
  const editExistingTask = (task) => {
    setTasks(tasks.map(t => (t.id === task.id ? task : t)));
    setEditTask(null);
  };

  return (
    <div>
      <Button onClick={() => setModalOpen(true)}>Add Task</Button>
      <Table>
        <Table.Header>
          <Table.Column>Task</Table.Column>
          <Table.Column>Description</Table.Column>
          <Table.Column>Actions</Table.Column>
        </Table.Header>
        <Table.Body>
          {tasks.map(task => (
            <Table.Row key={task.id}>
              <Table.Cell>{task.name}</Table.Cell>
              <Table.Cell>{task.description}</Table.Cell>
              <Table.Cell>
                <Button onClick={() => setEditTask(task)}>Edit</Button>
                <Button onClick={() => deleteTask(task.id)}>Delete</Button>
              </Table.Cell>
            </Table.Row>
          ))}
        </Table.Body>
      </Table>
      {modalOpen && (
        <Modal open={modalOpen} onClose={() => setModalOpen(false)}>
          <Modal.Header>Add Task</Modal.Header>
          <Modal.Body>
            <Input placeholder="Task Name" onChange={e => setEditTask({ ...editTask, name: e.target.value })} />
            <Input placeholder="Description" onChange={e => setEditTask({ ...editTask, description: e.target.value })} />
          </Modal.Body>
          <Modal.Footer>
            <Button onClick={() => { addTask(editTask); setModalOpen(false); }}>Add</Button>
          </Modal.Footer>
        </Modal>
      )}
    </div>
  );
};

export default TaskManagement;
import UserManagement from '../components/UserManagement';
import Analytics from '../components/Analytics';
import Notifications from '../components/Notifications';
import Settings from '../components/Settings';
import TaskManagement from '../components/TaskManagement';

export default function Home() {
  const metrics = {
    userCount: 100,
    activeUsers: 50,
    revenue: 1000,
  };

  return (
    <div>
      <h1>Admin Dashboard</h1>
      <UserManagement />
      <Analytics metrics={metrics} />
      <Notifications />
      <Settings />
      <TaskManagement />
    </div>
  );
}
import { Button, Card, Text } from '@nextui-org/react';
import { useEffect } from 'react';

const Notifications = () => {
  useEffect(() => {
    if (Notification.permission !== 'granted') {
      Notification.requestPermission();
    }
  }, []);

  const sendNotification = () => {
    if (Notification.permission === 'granted') {
      new Notification('This is a test notification');
    }
  };

  return (
    <Card>
      <Text>Notifications</Text>
      <Button onClick={sendNotification}>Send Notification</Button>
    </Card>
  );
};

export default Notifications;
npm run dev
http://localhost:3000
