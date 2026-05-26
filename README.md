# SusCraft

SusCraft là plugin minigame cho Paper 1.21.4, mô phỏng lối chơi kiểu Among Us trong Minecraft: tạo phòng, join phòng, chia role Crewmate/Impostor, làm task, kill, report xác, họp, vote, sabotage, vent, camera/security, admin table và hệ thống ghost.

Plugin ưu tiên an toàn dữ liệu người chơi: trước khi vào game sẽ snapshot inventory, armor, offhand, XP, máu, food, gamemode, vị trí, effect, scoreboard và các trạng thái quan trọng khác. Khi game kết thúc, người chơi rời game, server reload hoặc plugin disable, plugin sẽ cố gắng restore lại trạng thái cũ.

## Yêu Cầu

| Thành phần | Phiên bản |
| --- | --- |
| Java | 21 |
| Server | Paper 1.21.4 |
| API | Paper API |

Plugin có hỗ trợ optional:

| Plugin | Trạng thái | Ghi chú |
| --- | --- | --- |
| PlaceholderAPI | Optional | Dùng placeholder trong message nếu có |
| LuckPerms | Optional | Dùng permission Bukkit bình thường, tương thích LuckPerms |
| Vault | Optional | Có detect, chưa có reward economy mặc định |

## Cài Đặt Lên Server

1. Copy file `target/suscraft-1.0.0.jar` vào thư mục `plugins` của server Paper.
2. Khởi động server.
3. Plugin sẽ tự tạo các file cấu hình:

```text
plugins/SusCraft/config.yml
plugins/SusCraft/messages.yml
plugins/SusCraft/arenas.yml
plugins/SusCraft/pending-restore.yml
```

4. Chỉnh `config.yml` nếu cần.
5. Dùng lệnh `/susadmin reload` để reload config/message/arena sau khi chỉnh file.

## Permission

| Permission | Mặc định | Mô tả |
| --- | --- | --- |
| `suscraft.play` | true | Cho phép dùng lệnh người chơi |
| `suscraft.host` | true | Cho phép tạo phòng và làm host |
| `suscraft.admin` | op | Cho phép setup arena và bypass quyền host |

Nếu dùng LuckPerms:

```text
/lp group default permission set suscraft.play true
/lp group default permission set suscraft.host true
/lp group admin permission set suscraft.admin true
```

## Lệnh Người Chơi

| Lệnh | Mô tả |
| --- | --- |
| `/sus help` | Xem trợ giúp |
| `/sus create <arena>` | Tạo room mới từ arena |
| `/sus join <roomId>` | Join room |
| `/sus leave` | Rời room hoặc rời game |
| `/sus start` | Host bắt đầu countdown |
| `/sus invite <player>` | Host mời người chơi |
| `/sus kick <player>` | Host kick người chơi trong phòng chờ |
| `/sus impostors <amount>` | Host đặt số impostor |
| `/sus report` | Report xác gần nhất |
| `/sus vote <player\|skip>` | Vote người chơi hoặc skip |
| `/sus emergency` | Gọi họp khẩn tại emergency button |
| `/sus tasks` | Mở danh sách task |
| `/sus map` | Mở admin table cơ bản |

## Lệnh Admin

| Lệnh | Mô tả |
| --- | --- |
| `/susadmin help` | Xem trợ giúp admin |
| `/susadmin createarena <name>` | Tạo arena |
| `/susadmin deletearena <name>` | Xóa arena |
| `/susadmin list` | Xem danh sách arena |
| `/susadmin info <arena>` | Xem thông tin arena |
| `/susadmin setlobby <arena>` | Set lobby chờ |
| `/susadmin setspawn <arena>` | Thêm spawn trong game |
| `/susadmin setmeeting <arena>` | Set vị trí họp |
| `/susadmin setend <arena>` | Set vị trí kết thúc |
| `/susadmin setemergency <arena>` | Set vị trí nút họp khẩn |
| `/susadmin addtask <arena> <taskType> <taskId>` | Thêm task point tại vị trí đứng |
| `/susadmin removetask <arena> <taskId>` | Xóa task point |
| `/susadmin addsabotage <arena> <type> <id>` | Thêm điểm sửa sabotage |
| `/susadmin removesabotage <arena> <id>` | Xóa điểm sửa sabotage |
| `/susadmin addvent <arena> <group> <id>` | Thêm vent vào group |
| `/susadmin removevent <arena> <id>` | Xóa vent |
| `/susadmin addcamera <arena> <roomName> <id>` | Thêm camera/security point |
| `/susadmin removecamera <arena> <id>` | Xóa camera |
| `/susadmin addroomregion <arena> <roomName> <radius>` | Thêm vùng phòng |
| `/susadmin removeroomregion <arena> <roomName>` | Xóa vùng phòng |
| `/susadmin adddoor <arena> <doorId> <roomName>` | Thêm cửa sabotage bằng block đang nhìn |
| `/susadmin removedoor <arena> <doorId>` | Xóa cửa |
| `/susadmin validate <arena>` | Kiểm tra arena đã setup đủ chưa |
| `/susadmin reload` | Reload config, messages và arenas |

## Setup Arena Mẫu

Ví dụ arena tên `skeld`.

### 1. Tạo Arena

```text
/susadmin createarena skeld
```

### 2. Set Lobby Chờ

Đứng tại vị trí người chơi sẽ chờ trước khi game bắt đầu:

```text
/susadmin setlobby skeld
```

### 3. Thêm Spawn Trong Game

Đứng lần lượt tại nhiều vị trí spawn trong map và chạy:

```text
/susadmin setspawn skeld
/susadmin setspawn skeld
/susadmin setspawn skeld
/susadmin setspawn skeld
```

Nên có ít nhất bằng số người chơi tối đa, nhưng tối thiểu phải có 1 spawn.

### 4. Set Vị Trí Meeting

Đứng tại bàn họp hoặc vị trí tất cả người chơi sẽ được teleport về khi report/emergency:

```text
/susadmin setmeeting skeld
```

### 5. Set Vị Trí End

Đứng tại vị trí sau khi game kết thúc:

```text
/susadmin setend skeld
```

Hiện tại restore snapshot sẽ đưa người chơi về vị trí trước khi vào game. `endSpawn` vẫn được lưu để mở rộng hoặc dùng trong flow kết thúc sau này.

### 6. Set Emergency Button

Đứng tại vị trí nút emergency:

```text
/susadmin setemergency skeld
```

Khi chơi, người sống phải đứng gần điểm này và dùng `/sus emergency` hoặc item Emergency để gọi họp.

### 7. Thêm Task Point

Đứng tại từng vị trí task và thêm task:

```text
/susadmin addtask skeld WIRES wires_1
/susadmin addtask skeld BUTTON_SEQUENCE buttons_1
/susadmin addtask skeld UPLOAD_DATA upload_1
/susadmin addtask skeld FUEL_ENGINE fuel_1
/susadmin addtask skeld REACTOR_PATTERN reactor_task_1
/susadmin addtask skeld CARD_SWIPE card_1
/susadmin addtask skeld DOWNLOAD_UPLOAD data_1
/susadmin addtask skeld FIX_WIRING_MULTI wiring_multi_1
/susadmin addtask skeld CALIBRATE_DISTRIBUTOR distributor_1
/susadmin addtask skeld EMPTY_GARBAGE garbage_1
```

Các loại task hợp lệ:

```text
WIRES
BUTTON_SEQUENCE
UPLOAD_DATA
FUEL_ENGINE
REACTOR_PATTERN
CARD_SWIPE
DOWNLOAD_UPLOAD
FIX_WIRING_MULTI
CALIBRATE_DISTRIBUTOR
EMPTY_GARBAGE
```

Người chơi làm task bằng cách đứng gần task point và dùng item Task List hoặc click/interact gần vị trí task.

### 8. Thêm Điểm Sửa Sabotage

Đứng tại từng vị trí sửa sabotage:

```text
/susadmin addsabotage skeld LIGHTS lights_1
/susadmin addsabotage skeld OXYGEN oxy_1
/susadmin addsabotage skeld OXYGEN oxy_2
/susadmin addsabotage skeld REACTOR reactor_1
/susadmin addsabotage skeld REACTOR reactor_2
/susadmin addsabotage skeld COMMS comms_1
```

Các loại sabotage:

```text
LIGHTS
OXYGEN
REACTOR
COMMS
DOORS
```

`DOORS` dùng door point riêng, không dùng `addsabotage`.

### 9. Thêm Vent

Đứng tại vị trí vent thứ nhất:

```text
/susadmin addvent skeld main vent_1
```

Đứng tại vị trí vent thứ hai:

```text
/susadmin addvent skeld main vent_2
```

Đứng tại vị trí vent thứ ba:

```text
/susadmin addvent skeld main vent_3
```

Các vent cùng `group` sẽ teleport qua lại với nhau. Ví dụ `main` là một mạng vent chung.

### 10. Thêm Room Region

Room region dùng cho camera log và admin table. Đứng ở giữa phòng và đặt bán kính:

```text
/susadmin addroomregion skeld Cafeteria 8
/susadmin addroomregion skeld Electrical 6
/susadmin addroomregion skeld Reactor 6
/susadmin addroomregion skeld Security 5
```

Khi người chơi đi vào/ra vùng, plugin ghi log để camera/security hiển thị.

### 11. Thêm Camera

Đứng tại vị trí security/camera terminal:

```text
/susadmin addcamera skeld Cafeteria cam_cafeteria
/susadmin addcamera skeld Electrical cam_electrical
/susadmin addcamera skeld Reactor cam_reactor
```

Camera GUI hiển thị log di chuyển gần đây theo room.

### 12. Thêm Door Sabotage

Nhìn vào block cửa hoặc block muốn bị thay khi door sabotage rồi chạy:

```text
/susadmin adddoor skeld door_electrical Electrical
```

Khi impostor sabotage doors, block này sẽ đổi thành material trong config, mặc định là `BARRIER`, sau đó tự restore.

### 13. Validate Arena

Sau khi setup:

```text
/susadmin validate skeld
```

Arena hợp lệ bắt buộc có:

| Thành phần | Bắt buộc |
| --- | --- |
| Lobby spawn | Có |
| Meeting spawn | Có |
| Ít nhất 1 game spawn | Có |
| Ít nhất 1 task point | Có |
| Emergency button | Có |

Plugin sẽ cảnh báo nếu thiếu sabotage, vent, camera hoặc room region. Cảnh báo không nhất thiết chặn start, nhưng nên setup đủ để gameplay tốt hơn.

## Cách Chơi

### 1. Host Tạo Room

```text
/sus create skeld
```

Plugin trả về room ID, ví dụ:

```text
ABCDE
```

### 2. Người Chơi Join Room

```text
/sus join ABCDE
```

Một người chỉ được ở một room tại cùng thời điểm.

### 3. Host Chọn Số Impostor

```text
/sus impostors 1
```

Giá trị phải hợp lệ theo `config.yml`:

```yaml
game:
  max-impostors: 3
```

### 4. Host Start Game

```text
/sus start
```

Plugin sẽ kiểm tra:

| Điều kiện | Mô tả |
| --- | --- |
| Arena hợp lệ | Dùng `/susadmin validate <arena>` để kiểm tra |
| Đủ người chơi | Theo `game.min-players` |
| Không quá max player | Theo `game.max-players` |
| Số impostor hợp lệ | Ít nhất 1 và không vượt giới hạn |

Nếu hợp lệ, countdown bắt đầu. Nếu trong countdown thiếu người chơi, countdown bị hủy.

## Role Và Item Trong Game

### Crewmate

Crewmate nhận các item:

| Item | Công dụng |
| --- | --- |
| Task List | Xem task hoặc mở task gần nhất |
| Report | Report xác gần nhất |
| Emergency | Gọi họp khẩn khi đứng gần emergency button |
| Ship Map | Mở admin table |

Mục tiêu:

| Mục tiêu | Cách thắng |
| --- | --- |
| Hoàn thành task | Task bar đạt 100% |
| Loại impostor | Vote/eject toàn bộ impostor |

### Impostor

Impostor nhận các item:

| Item | Công dụng |
| --- | --- |
| Kill | Giết crewmate gần nhất trong range |
| Sabotage | Mở GUI sabotage |
| Vent | Mở GUI vent gần nhất |
| Fake Task List | Xem/làm fake task |
| Report | Có thể report xác |
| Ship Map | Mở admin table |

Mục tiêu:

| Mục tiêu | Cách thắng |
| --- | --- |
| Parity | Số impostor sống >= số crewmate sống |
| OXY timeout | OXY không được sửa kịp |
| Reactor timeout | Reactor không được sửa kịp |

### Ghost

Khi chết hoặc bị eject, người chơi thành ghost.

Ghost có các rule:

| Rule | Mô tả |
| --- | --- |
| Người sống không thấy ghost | Plugin dùng visibility API |
| Ghost thấy ghost khác | Ghost vẫn thấy nhau |
| Ghost không vote | Chỉ người sống được vote |
| Ghost không report | Chỉ người sống report được |
| Ghost không pickup/drop/phá/đặt block | Plugin chặn event |
| Ghost crewmate có thể làm task | Bật/tắt bằng `tasks.ghost-can-do-task` |
| Ghost impostor có thể sabotage | Bật/tắt bằng `impostor.can-sabotage-as-ghost` |

## Task System

Mỗi người chơi được assign task theo:

```yaml
tasks:
  tasks-per-player: 5
```

Cách làm task:

1. Đứng gần task point đã setup.
2. Dùng item Task List hoặc click/interact gần vị trí task.
3. GUI task mở ra.
4. Click nút ở giữa GUI để tăng progress.
5. Khi đủ progress, task hoàn thành và task bar tăng.

Task hiện tại là bản Minecraft GUI thay thế hợp lý cho Among Us. Mỗi loại task có GUI riêng, nhưng logic ổn định theo progress/click để tránh lỗi kẹt task.

## Kill Và Report

### Kill

Impostor có thể kill bằng:

| Cách | Mô tả |
| --- | --- |
| Item Kill | Dùng item Kill khi đứng gần mục tiêu |
| Đánh trực tiếp | Đánh crewmate, plugin xử lý như kill nếu hợp lệ |

Điều kiện kill:

| Điều kiện | Config |
| --- | --- |
| Cooldown | `impostor.kill-cooldown` |
| Range | `impostor.kill-range` |
| Line of sight | `impostor.require-line-of-sight` |
| Không trong meeting | Luôn chặn |
| Không kill ghost | Luôn chặn |
| Không kill impostor khác | Luôn chặn |

Sau khi kill:

| Kết quả | Mô tả |
| --- | --- |
| Victim thành ghost | Người sống không nhìn thấy |
| Tạo xác | ArmorStand tại vị trí chết |
| Impostor vào cooldown | Theo config |
| Check win condition | Gọi ngay sau kill |

### Report

Cách report:

```text
/sus report
```

Hoặc dùng item Report gần xác.

Điều kiện:

| Điều kiện | Mô tả |
| --- | --- |
| Người report còn sống | Ghost không report được |
| Đứng gần xác | Theo `mechanics.report-range` |
| Không đang meeting | Meeting đang chạy thì chặn |

Khi report:

1. Xóa toàn bộ xác đang có.
2. Hủy sabotage active.
3. Teleport người chơi về meeting spawn.
4. Freeze movement.
5. Bắt đầu discussion timer.
6. Sau discussion mở vote GUI.

## Meeting Và Vote

Flow meeting:

| State | Mô tả |
| --- | --- |
| `MEETING_DISCUSSION` | Người chơi chat thảo luận |
| `MEETING_VOTING` | Mở GUI vote |
| `MEETING_RESULT` | Hiện kết quả vote |
| `PLAYING` | Quay lại game |

Config timer:

```yaml
meeting:
  discussion-time: 30
  voting-time: 45
  result-time: 5
```

Cách vote:

```text
/sus vote <player>
/sus vote skip
```

Hoặc click trong Vote GUI.

Kết quả vote:

| Trường hợp | Kết quả |
| --- | --- |
| Một player cao nhất | Player bị eject thành ghost |
| Skip cao nhất | Không ai bị eject |
| Hòa phiếu | Không ai bị eject |
| Không có vote | Không ai bị eject |

Nếu bật:

```yaml
game:
  confirm-ejects: true
```

Plugin sẽ thông báo người bị eject có phải impostor hay không.

## Sabotage

Impostor dùng item Sabotage để mở GUI.

### Lights

Hiệu ứng:

| Phe | Hiệu ứng |
| --- | --- |
| Crewmate sống | Blindness |
| Impostor | Không bị blindness |

Sửa bằng cách đứng gần sabotage point `LIGHTS` và click repair GUI.

### Oxygen

OXY có countdown:

```yaml
sabotage:
  oxygen-countdown: 45
```

Nếu hết giờ, impostor thắng.

Nên setup 2 điểm OXY:

```text
/susadmin addsabotage skeld OXYGEN oxy_1
/susadmin addsabotage skeld OXYGEN oxy_2
```

### Reactor

Reactor có countdown:

```yaml
sabotage:
  reactor-countdown: 45
```

Nếu hết giờ, impostor thắng.

Nên setup 2 điểm Reactor:

```text
/susadmin addsabotage skeld REACTOR reactor_1
/susadmin addsabotage skeld REACTOR reactor_2
```

### Comms

Comms sabotage làm camera/admin table bị vô hiệu nếu config bật:

```yaml
camera:
  disabled-by-comms: true

admin-table:
  disabled-by-comms: true
```

### Doors

Door sabotage đổi block cửa thành material đóng:

```yaml
doors:
  closed-material: BARRIER
```

Sau thời gian:

```yaml
sabotage:
  doors-duration: 10
```

Plugin tự restore block cũ.

### Chỉ Dẫn Điểm Hư Bằng Mũi Tên

Khi có sabotage đang hoạt động, người chơi phe thuyền viên còn sống sẽ thấy chỉ dẫn trên actionbar để tìm điểm cần sửa gần nhất.

Ví dụ hiển thị:

```text
Sửa Oxy: ↑ 32m (còn 2 điểm)
```

Ý nghĩa:

| Thành phần | Mô tả |
| --- | --- |
| `Sửa Oxy` | Loại sabotage đang cần sửa |
| `↑` | Mũi tên chỉ hướng theo góc nhìn hiện tại của người chơi |
| `32m` | Khoảng cách tới điểm sửa gần nhất |
| `còn 2 điểm` | Số điểm sửa sabotage còn chưa hoàn thành |

Các mũi tên có thể hiển thị:

```text
↑ ↗ → ↘ ↓ ↙ ← ↖
```

Mũi tên được tính theo hướng người chơi đang nhìn. Nếu thấy `↑` nghĩa là đi thẳng theo hướng đang nhìn. Nếu thấy `→` nghĩa là quay sang phải. Nếu thấy `↓` nghĩa là điểm sửa đang ở phía sau.

Mặc định chỉ thuyền viên còn sống thấy chỉ dẫn này. Impostor và ghost không thấy để tránh lộ thông tin không cần thiết, nhưng có thể bật trong config.

## Vent

Impostor dùng item Vent khi đứng gần vent point.

Điều kiện:

| Điều kiện | Mô tả |
| --- | --- |
| Chỉ impostor sống | Crewmate không dùng vent |
| Phải đứng gần vent | Theo `mechanics.interact-range` |
| Cùng group | Chỉ teleport giữa vent cùng group |
| Có cooldown | Theo `mechanics.vent-cooldown` |

Ví dụ:

```text
/susadmin addvent skeld main vent_1
/susadmin addvent skeld main vent_2
```

`vent_1` và `vent_2` cùng group `main`, nên có thể teleport qua lại.

## Camera Và Admin Table

### Room Region

Room region là vùng dùng để đếm player và ghi log di chuyển:

```text
/susadmin addroomregion skeld Electrical 6
```

Plugin ghi log khi người chơi vào/ra vùng. PlayerMoveEvent được throttle để tránh xử lý quá nặng.

### Camera

Camera hiển thị log di chuyển gần đây:

```text
/susadmin addcamera skeld Electrical cam_electrical
```

Khi người chơi đứng gần camera point, click/interact để mở camera GUI.

Config thời gian log:

```yaml
camera:
  log-duration-seconds: 60
```

### Admin Table

Admin table mở bằng:

```text
/sus map
```

Hoặc item Ship Map.

GUI chỉ hiển thị số người trong từng room, không hiện tên.

## Cấu Hình Quan Trọng

### Game

```yaml
game:
  min-players: 4
  max-players: 15
  default-impostors: 1
  max-impostors: 3
  starting-countdown: 10
  confirm-ejects: true
  hide-nametags: true
  isolate-chat: true
  reconnect-timeout-seconds: 60
```

### Task

```yaml
tasks:
  tasks-per-player: 5
  ghost-can-do-task: true
  taskbar-enabled: true
```

### Impostor

```yaml
impostor:
  kill-cooldown: 30
  kill-range: 3.0
  require-line-of-sight: true
  reset-cooldown-after-meeting: true
  can-sabotage-as-ghost: false
```

### Meeting

```yaml
meeting:
  discussion-time: 30
  voting-time: 45
  result-time: 5
  emergency-meetings-per-player: 1
  allow-emergency-during-sabotage: false
```

### Sabotage

```yaml
sabotage:
  global-cooldown: 25
  lights-duration-max: 120
  oxygen-countdown: 45
  reactor-countdown: 45
  comms-duration-max: 120
  doors-duration: 10
```

### Điều Hướng Sabotage

```yaml
navigation:
  sabotage-arrow-enabled: true
  show-to-impostors: false
  show-to-ghosts: false
  format: "&cSửa {type}: &e{arrow} &f{distance}m &7(còn {remaining} điểm)"
```

Placeholder trong `format`:

| Placeholder | Mô tả |
| --- | --- |
| `{type}` | Tên sabotage đã Việt hóa, ví dụ `Oxy`, `Đèn`, `Lò phản ứng` |
| `{arrow}` | Mũi tên định hướng |
| `{distance}` | Khoảng cách tới điểm sửa gần nhất, tính bằng block/met |
| `{remaining}` | Số điểm sửa còn lại |

Gợi ý cấu hình:

| Mục | Khuyến nghị |
| --- | --- |
| `sabotage-arrow-enabled` | Để `true` nếu map lớn hoặc nhiều phòng |
| `show-to-impostors` | Nên để `false` để impostor không được chỉ đường sửa |
| `show-to-ghosts` | Nên để `false` nếu không muốn ghost dẫn đường cho người sống |

## Chat

Nếu bật:

```yaml
game:
  isolate-chat: true
```

Chat được tách khỏi người ngoài game.

Rule mặc định:

| Trạng thái | Chat |
| --- | --- |
| Ngoài meeting | Người sống không chat với nhau |
| Trong meeting | Người sống chat được |
| Ghost | Chỉ ghost thấy chat ghost |
| Người ngoài game | Không thấy chat trong game |

## Reconnect Và Restore

Khi người chơi mất kết nối trong game:

1. Plugin mark player là disconnected.
2. Player có thời gian reconnect theo config:

```yaml
game:
  reconnect-timeout-seconds: 60
```

3. Nếu join lại kịp, plugin đưa player vào lại room với role/state hiện tại.
4. Nếu hết timeout, plugin xử lý player như rời game/chết và lưu snapshot nếu cần.

Khi player offline lúc game kết thúc, snapshot được lưu vào:

```text
plugins/SusCraft/pending-restore.yml
```

Khi player join lại, plugin tự restore pending snapshot.

## An Toàn Inventory

Plugin xử lý các điểm sau:

| Trường hợp | Cách xử lý |
| --- | --- |
| Trước khi vào game | Capture snapshot đầy đủ |
| Khi vào game | Clear inventory rồi give item game |
| Item game | Gắn PDC key `suscraft:item_type` và `suscraft:room_id` |
| Drop item game | Bị chặn |
| Move item game trong inventory/chest | Bị chặn |
| Death drop | Bị clear |
| End game | Xóa item game trước, restore snapshot sau |
| Player offline khi end | Lưu pending restore |
| Plugin disable | Cleanup room, restore online, lưu pending offline |

Không nên dùng `/reload` của Bukkit/Paper trên production. Nếu bắt buộc reload, plugin vẫn cố cleanup như disable, nhưng cách an toàn hơn là restart server.

## Quy Trình Test Nhanh

Sau khi setup arena:

1. `/susadmin validate skeld`
2. Player A: `/sus create skeld`
3. Player B/C/D: `/sus join <roomId>`
4. Host: `/sus impostors 1`
5. Host: `/sus start`
6. Kiểm tra mỗi player nhận role và item.
7. Crewmate đứng gần task, dùng Task List, hoàn thành task.
8. Impostor dùng Kill gần crewmate.
9. Người sống dùng Report gần xác.
10. Vote bằng GUI hoặc `/sus vote <player>`.
11. Test sabotage bằng item Sabotage.
12. Test vent bằng item Vent gần vent point.
13. Kết thúc game và kiểm tra inventory/vị trí/XP/gamemode được restore.

## Lỗi Thường Gặp

### Không Start Được Game

Chạy:

```text
/susadmin validate <arena>
```

Kiểm tra các lỗi thường gặp:

| Lỗi | Cách sửa |
| --- | --- |
| Missing lobbySpawn | Dùng `/susadmin setlobby <arena>` |
| Missing meetingSpawn | Dùng `/susadmin setmeeting <arena>` |
| Missing gameSpawns | Dùng `/susadmin setspawn <arena>` |
| Missing taskPoints | Thêm task bằng `/susadmin addtask` |
| Missing emergencyButtonLocation | Dùng `/susadmin setemergency <arena>` |
| Not enough players | Tăng người chơi hoặc giảm `game.min-players` |
| Invalid impostor amount | Dùng `/sus impostors <amount>` hợp lệ |

### Không Report Được Xác

Kiểm tra:

| Nguyên nhân | Cách xử lý |
| --- | --- |
| Đứng quá xa xác | Lại gần hơn hoặc tăng `mechanics.report-range` |
| Player là ghost | Ghost không report được |
| Đang meeting | Chờ meeting kết thúc |
| Xác đã bị xóa | Meeting trước đó đã clear bodies |

### Không Kill Được

Kiểm tra:

| Nguyên nhân | Cách xử lý |
| --- | --- |
| Đang cooldown | Chờ hết cooldown |
| Ngoài range | Tăng `impostor.kill-range` hoặc đứng gần hơn |
| Không có line of sight | Tắt `impostor.require-line-of-sight` nếu muốn |
| Target là impostor/ghost | Không thể kill |
| Đang meeting | Kill bị chặn |

### Sabotage Không Sửa Được

Kiểm tra:

| Nguyên nhân | Cách xử lý |
| --- | --- |
| Thiếu sabotage point | Dùng `/susadmin addsabotage` |
| Sai type point | Ví dụ OXY cần point `OXYGEN` |
| Đứng quá xa | Lại gần hơn hoặc tăng `mechanics.interact-range` |
| Sabotage đã hết | Countdown timeout hoặc đã được sửa |

### Không Restore Inventory

Kiểm tra file:

```text
plugins/SusCraft/pending-restore.yml
```

Nếu player offline khi game end, plugin sẽ restore khi player join lại. Nếu vẫn lỗi, xem console để biết snapshot nào không restore được.

## Ghi Chú Vận Hành

- Không chỉnh `arenas.yml` thủ công nếu không cần. Nên dùng `/susadmin`.
- Sau khi setup map, luôn chạy `/susadmin validate <arena>`.
- Với map lớn, nên tạo room region bán kính vừa đủ, tránh overlap quá nhiều.
- Door sabotage thay block thật trong map, nhưng plugin có lưu block cũ và restore khi hết sabotage/end/disable.
- Nếu server crash cứng, block door đang bị đổi có thể cần sửa thủ công vì plugin không có cơ hội chạy cleanup.
- Nên test bằng ít nhất 4 tài khoản hoặc bot/player giả trước khi mở public.
