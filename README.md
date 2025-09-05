# IRC Server (ft_irc)

A minimal Internet Relay Chat (IRC) server written in C++ that implements a practical subset of RFC 1459/2812. This project was built as part of 42 Network’s ft_irc curriculum.

- Repository: [otmansabir/irc](https://github.com/otmansabir/irc)
- Language: C++
- Status: Educational, single-binary server

## Team

- [Otmansabir](https://github.com/otmansabir)
- [Faouzi El Hazzat](https://github.com/Faouzi-7)
- [Mehdi Lahlafi](https://github.com/ryozakii)

## Features

Core server capabilities:
- Multi-client TCP server with non-blocking I/O
- IRC user registration and authentication (PASS/NICK/USER)
- Channel lifecycle and membership (JOIN/PART)
- Messaging:
  - Private messages (PRIVMSG user)
  - Channel messages (PRIVMSG #channel)
- Channel moderation:
  - KICK
  - TOPIC
  - INVITE
  - MODE (common modes like +i, +t, +k, +l, +o when applicable)
- Numeric replies per RFC style (see `reply.hpp`)

Extras:
- Simple demonstration bot client (see `ircbot.cpp`) — optional

Note: This is a learning project that focuses on a well-chosen subset of the IRC spec. Behavior aims to be compliant with common clients but may not cover every edge case or command.

## Getting Started

### Requirements

- A C++ compiler (e.g., clang++, g++)
- Make
- POSIX-compatible system (Linux/macOS)

### Build

```bash
make
```

Common Makefile targets:
- `make` or `make all` — build the server
- `make clean` — remove object files
- `make fclean` — remove objects and binary
- `make re` — rebuild from scratch

### Run

```bash
./ircserv <port> <password>
```

Examples:
- Run on default IRC port with password “secret”:
  ```bash
  ./ircserv 6667 secret
  ```

The server listens on the given TCP port and expects clients to authenticate with the provided server password.

## Connect with a Client

You can use a real IRC client (irssi/WeeChat/HexChat) or raw tools like `nc` (netcat).

### Quick test with netcat

```bash
# In one terminal, run the server
./ircserv 6667 secret

# In another terminal, connect:
nc 127.0.0.1 6667

# Then type the registration sequence:
PASS secret
NICK alice
USER alice 0 * :Alice Example

# Create or join a channel and send messages
JOIN #42
PRIVMSG #42 :Hello, world!
TOPIC #42 :Welcome to our channel
PART #42 :bye
QUIT :see you
```

### With a real IRC client (example flow)

- Start your IRC client and connect to 127.0.0.1 on the chosen port.
- Provide the server password you started the server with.
- Set your nickname, then join channels and chat as usual.

Client-specific commands vary; consult your client’s documentation for the exact `/connect` syntax and how to set a server password.

## Project Structure

Key files in this repository:
- `main.cpp` — entry point, server bootstrap
- `Ircserv.hpp` / `Ircserv.cpp` — core server class and event loop
- `client.hpp` — client/session representation
- `channel.hpp` / `channel.cpp` — channel data structure and logic
- `authentication.cpp` — PASS/NICK/USER flow and registration
- `join.cpp`, `part.cpp`, `kick.cpp`, `invite.cpp`, `topic.cpp` — respective IRC command handlers
- `mode.cpp` — channel/user mode handling
- `privmsg.cpp` — message dispatching
- `tools.cpp` — helper utilities
- `reply.hpp` — numeric reply constants and helpers
- `ircbot.cpp` — simple example bot client (optional/demo)
- `Makefile` — build configuration

## Example IRC Session

```irc
# Client to server:
PASS secret
NICK bob
USER bob 0 * :Bob Builder

# Join and chat
JOIN #dev
PRIVMSG #dev :hello from bob

# Set topic (if allowed)
TOPIC #dev :Development channel

# Moderate (if you have operator/rights)
MODE #dev +o bob
INVITE alice #dev
KICK #dev troublemaker :rules!

# Leave
PART #dev :bye
QUIT :goodbye
```

## Notes on Modes (subset)

Commonly implemented channel modes:
- `+i` — invite-only channel
- `+t` — only channel operators can set the topic
- `+k <key>` — channel password
- `+l <limit>` — user limit
- `+o <nick>` — give/take operator privileges

Exact availability and semantics follow what’s implemented in `mode.cpp` for this project’s scope.

## Bot (optional)

A simple demonstration bot is provided in `ircbot.cpp`. It can be used as a reference for writing IRC clients or as a test peer. Build/usage may vary; check the source and Makefile if a dedicated target is provided, or compile it manually.

## Troubleshooting

- Can’t connect? Ensure the port is open and not in use, and that you’re connecting with the correct server password (the one you started the server with).
- Registration issues? The typical order is `PASS` (if required) → `NICK` → `USER`.
- Client compatibility: While common IRC clients should work, some advanced features may not be supported in this educational implementation.

## References

- RFC 1459 (Original IRC Protocol)
- RFC 2812 (IRC Client Protocol)
- 42 Network ft_irc subject

## License

No license specified. This project is provided for educational purposes.

## Acknowledgments

Thanks to the 42 Network community and maintainers for guidance and resources.
