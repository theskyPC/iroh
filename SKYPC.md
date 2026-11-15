# skyPC fork of iroh

Branch `skypc-v1.1.0` is upstream iroh v1.1.0 (`fddf1a4`) plus four patches
skyPC needs, one commit each:

1. **Re-add custom transport addresses as backup paths.** When a client
   connection comes up, iroh re-opens known relay addresses as backup paths;
   this also re-opens custom transport addresses, so a connection that starts
   direct keeps the skyPC UDP relay as a standby path.
2. **Reopen abandoned custom transport paths.** An abandoned custom path is
   reopened after 1 s on client connections, since no holepunching brings it
   back (for example after a relay restart).
3. **QAD-probe every relay.** Probes went only to the first five relays in URL
   order, so later relays were never measured and could never become the home
   relay.
4. **Expose QUIC receive timestamps.**
   `QuicTransportConfigBuilder::max_receive_timestamps_per_ack` turns on
   the Receive Timestamps extension of skyPC's noq fork (theskyPC/noq,
   patched in here), which skyPC's GoogCC congestion controller times
   packets by.

Upstream 1.3.0 has neither patch 1 nor 2. Patch 3 is unchecked against 1.3.0.
Patch 4 needs the noq fork.
To upgrade, rebase these commits onto the new release tag.
