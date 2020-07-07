# Nerves.IO.PN532

## Hardware

Any PN532 board should work as long as it supports UART, I've been using the following board to develop.

## Installation

This version is best pulled from git: 

  1. Add `nerves_io_pn532` to your list of dependencies in `mix.exs`:

   ```elixir
   def deps do
    [{:nerves_io_pn532, git: "https://github.com/dwyl/nerves_io_pn532"}]
   end
   ```

## How to use

```elixir
defmodule MifareClientImplementation do
  use Nerves.IO.PN532.MifareClient

  def handle_event(:card_detected, card = %{tg: target_number, sens_res: sens_res, sel_res: sel_res, nfcid: identifier}) do
    Logger.info("Detected new Mifare card with ID: #{Base.encode16(identifier)}")
  end

  def handle_event(:card_lost, card = %{tg: target_number, sens_res: sens_res, sel_res: sel_res, nfcid: identifier}) do
    Logger.info("Lost connection with Mifare card with ID: #{Base.encode16(identifier)}")
  end
end
```

```elixir
defmodule Example do
  def main do
    with {:ok, pid} <- MifareClientImplementation.start_link(),
         :ok <- MifareClientImplementation.open(pid, "COM3"),
         :ok <- MifareClientImplementation.start_target_detection(pid) do
      # ...
    end
  end
end
```
